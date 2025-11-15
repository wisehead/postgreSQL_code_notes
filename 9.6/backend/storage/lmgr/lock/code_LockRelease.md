#1.LockRelease

```cpp
/* LockRelease -- look up 'locktag' and release one 'lockmode' lock on it.
 *      Release a session lock if 'sessionLock' is true, else release a regular transaction lock.
 *
 * Side Effects: find any waiting processes that are now wakable, grant them their requested locks and awaken them.
 *        (We have to grant the lock here to avoid a race between the waking process and any new process to
 *        come along and request the lock.) */
bool
LockRelease(const LOCKTAG *locktag, LOCKMODE lockmode, bool sessionLock)
//在lockmode对象上释放lockmode指定粒度的锁
{...//从本地锁表中找要释放的锁
    locallock = (LOCALLOCK *) hash_search(LockMethodLocalHash, (void *)
 &localtag, HASH_FIND, NULL);
... //没有找到则报错
    {   //处理ResourceOwner和Lock的关系
...
    }
    locallock->nLocks--; //持锁次数递减，如果值大于零，表示这个锁还需要继续保持
                             （还有其他粒度的锁存在）
    if (locallock->nLocks > 0)
        return TRUE;
    /* Attempt fast release of any lock eligible for the fast path. */
    if (EligibleForRelationFastPath(locktag, lockmode) && FastPathLocalUseCount > 0)
    //存在可快速释放的锁
    {...
        LWLockAcquire(&MyProc->backendLock, LW_EXCLUSIVE);
        released = FastPathUnGrantRelationLock(locktag->locktag_field2, lockmode);
        LWLockRelease(&MyProc->backendLock);
        if (released)
        {
            RemoveLocalLock(locallock);
            return TRUE;
        }
    }
...
    if (!lock) //从全局锁表中，找出lock和prolock两种对象
    {...
        lock = (LOCK *) hash_search_with_hash_value(LockMethodLockHash,
        (const void *) locktag, locallock->hashcode, HASH_FIND, NULL);
        if (!lock)
            elog(ERROR, "failed to re-find shared lock object");
...
        locallock->proclock = (PROCLOCK *) hash_search(LockMethodProcLockHash,
        (void *) &proclocktag, HASH_FIND, NULL);
        if (!locallock->proclock)
            elog(ERROR, "failed to re-find shared proclock object");
    }
... //找到之后，释放锁，并唤醒等待此锁的会话进程
    wakeupNeeded = UnGrantLock(lock, lockmode, proclock, lockMethodTable);找到之后，
   //释放锁。并决定是否需要唤醒其他等待此锁的会话进程（唤醒的条件是： 等待的队列中有申请
   此处需要释放的锁的类型）
    CleanUpLock(lock, proclock, lockMethodTable, locallock->hashcode, wakeupNeeded);
    //释放锁之后，唤醒等待此锁的会话进程
...
    RemoveLocalLock(locallock); //从本地锁表中去掉此锁请求
    return TRUE;
}
```