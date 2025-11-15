#1.SetupLockInTable

```cpp
/*
 * Find or create LOCK and PROCLOCK objects as needed for a new lock request.
 *
 * Returns the PROCLOCK object, or NULL if we failed to create the objects for
   lack of shared memory.
 * The appropriate partition lock must be held at entry, and will be held at exit. */
static PROCLOCK *
SetupLockInTable(LockMethod lockMethodTable, PGPROC *proc,
                 const LOCKTAG *locktag, uint32 hashcode, LOCKMODE lockmode)
//指定锁请求和hash值，查找是否存在对应的锁
{...
    //Find or create a lock with this tag.     从全局锁表LockMethodLockHash里找
    lock = (LOCK *) hash_search_with_hash_value(LockMethodLockHash,
    (const void *) locktag, hashcode, HASH_ENTER_NULL, &found);
    if (!lock)  //全局锁表LockMethodLockHash已满
        return NULL;
    if (!found)  //没有找到但是创建成功，需要初始化
    {
        lock->grantMask = 0;
        lock->waitMask = 0;
        SHMQueueInit(&(lock->procLocks));
        ProcQueueInit(&(lock->waitProcs));
...
    }
    else  //找到
    {...}
...
    //锁对象找到以后，则需要建立锁和进程之间的关系
    proclock = (PROCLOCK *) hash_search_with_hash_value(LockMethodProcLockHash,
                       (void *) &proclocktag,  proclock_hashcode, HASH_ENTER_NULL, &found);
    if (!proclock) //共享缓冲进程太多，队列满
    {
        if (lock->nRequested == 0) //锁还没有被使用，则从全局锁表中移除，
        因为LockMethodProcLockHash已经满了，得不到锁对应的proclock
        {...
            if (!hash_search_with_hash_value(LockMethodLockHash, (void *) &
           (lock->tag), hashcode, HASH_REMOVE, NULL))
                elog(PANIC, "lock table corrupted");
        }
        return NULL;
    }
    if (!found) //新建的prolock对象，初始化这个对象
    {...
        SHMQueueInsertBefore(&lock->procLocks, &proclock->lockLink);
        SHMQueueInsertBefore(&(proc->myProcLocks[partition]),  &proclock->procLink);
        PROCLOCK_PRINT("LockAcquire: new", proclock);
    }
    else   //根据宏的定义情况，确定是否进行可能的死锁检测，但不是PostgreSQL默认通过的死锁检测方式
    {...}
    lock->nRequested++;  //锁申请成功，则开始计数
    lock->requested[lockmode]++;
...
    return proclock;
}
```