#1.LockAcquireExtended

```cpp
/*
 * LockAcquireExtended - allows us to specify additional options
 *
 * reportMemoryError specifies whether a lock request that fills the lock table should generate an ERROR or not.
 * This allows a priority caller to note that the lock table is full and then begin taking extreme action
 * to reduce the number of other lock holders before retrying the action. */
LockAcquireResult
LockAcquireExtended(const LOCKTAG *locktag, LOCKMODE lockmode,
//这两个参数表示要在哪个对象上施加什么样的锁
                     bool sessionLock,  bool dontWait, bool reportMemoryError)
{...//第一步：从本地锁表中找锁
    locallock = (LOCALLOCK *) hash_search(LockMethodLocalHash, (void *)
 &localtag, HASH_ENTER, &found);
    //如果没有找到，则是一个新的本地锁对象，初始化此锁；否则，如果本地锁表满，则扩容一倍
...
    if (lockmode >= AccessExclusiveLock && locktag->locktag_type == LOCKTAG_RELATION &&
        !RecoveryInProgress() && XLogStandbyInfoActive()) //在关系上施加锁，需要同步
        到“a standby server”
    {
        LogAccessExclusiveLockPrepare();
        log_lock = true;
    }
    //第二步：用快速方法找锁
    //进程的结构体（参见8.2.2节对于进程结构体的说明）上提供了一些事务锁槽，用以加快事务的锁操
    作。获取事务锁时，优先使用在进程结构体上
    if (EligibleForRelationFastPath(locktag, lockmode) && FastPathLocalUseCount
    < FP_LOCK_SLOTS_PER_BACKEND)   //提供的快速访问锁的机制
    {...
        if (FastPathStrongRelationLocks->count[fasthashcode] != 0)
            acquired = false;
        else
            acquired = FastPathGrantRelationLock(locktag->locktag_field2, lockmode);
             //是否可以快速获得锁
        LWLockRelease(&MyProc->backendLock);
        if (acquired) //如果快速模式可用，则调用GrantLockLocal()函数赋予锁
        {
            locallock->lock = NULL;
            locallock->proclock = NULL;
            GrantLockLocal(locallock, owner); //locallock是注册在本地锁表里的，如本函
数初始之处。本函数的作用就是把准备授予的本地锁的属性修改，让锁上的计数器和锁的拥有者的计数器都增
长，且建立锁属主和锁的关系（解锁和判断死锁时候要使用）
            return LOCKACQUIRE_OK;
        }
    }
    //第三步：用快速方法到其他进程上找锁
    if (ConflictsWithRelationFastPath(locktag, lockmode))
   //想要申请的锁，其快速访问模式已经被其他进程抢先获得
    {...
        BeginStrongLockAcquire(locallock, fasthashcode);
        if (!FastPathTransferRelationLocks(lockMethodTable, locktag, hashcode))
        //如果不能分配（需在全局锁表中分配），则失败退出
        {...}
    }
... //第四步：在全局锁表上找锁，找不到则创建
    proclock = SetupLockInTable(lockMethodTable, MyProc, locktag, ashcode, lockmode);
    if (!proclock){...}              //申请失败，报错退出
    locallock->proclock = proclock;
     //申请成功，全局共享缓冲的锁表和进程等信息写入本地锁表的元素中，
    lock = proclock->tag.myLock;     //相当于同一个锁对象在本地锁表和全局锁表中都存在
    locallock->lock = lock;
    //第五步：检查是否存在锁冲突
    /* If lock requested conflicts with locks requested by waiters, must join wait queue.
     * Otherwise, check for conflict with already-held locks.
     (That's last because most complex check.) */
    if (lockMethodTable->conflictTab[lockmode] & lock->waitMask)
    //申请的锁与锁的等待者申请的锁冲突，则本次申请不可成功
        status = STATUS_FOUND;
    else
        status = LockCheckConflicts(lockMethodTable, lockmode, lock, proclock);
        //检查是否存在冲突，参见8.4.4节
    //第六步：如果锁不冲突则授予，如果有冲突则根据本函数的入口参数确定是否等待以及进行死锁检测
    if (status == STATUS_OK) //新申请的锁没有冲突，可以授予
    {
        /* No conflict with held or previously requested locks */
        GrantLock(lock, proclock, lockmode);  //新申请的锁的粒度即锁类型注册到锁对象上
        GrantLockLocal(locallock, owner);
   //锁上的计数器和锁的拥有者的计数器都增长，且建立锁属主和锁的关系（解锁和判断死锁时候要使用）
    }
    else   //新申请的锁有冲突，不可以授予
    {...
        if (dontWait){...}//申请不成功时指定不等待，执行一些清理工作后返回到上层函数。
         多数加锁申请都不等待，只有VACUUM等类操作需要等待
        //否则，申请不成功时指定等待
        MyProc->heldLocks = proclock->holdMask;
        //Set bitmask of locks this process already holds on this object.
...
        WaitOnLock(locallock, owner); // Sleep till someone wakes me up.
        //等待锁被授予。并进行死锁检测（参见8.5节）
...
    }
...
    return LOCKACQUIRE_OK;
}

```