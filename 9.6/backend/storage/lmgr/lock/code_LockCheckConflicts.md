#1.LockCheckConflicts

```cpp
/*
 * LockCheckConflicts -- test whether requested lock conflicts with those already granted
* Returns STATUS_FOUND if conflict, STATUS_OK if no conflict.
 *
 * NOTES:
 *        Here's what makes this complicated: one process's locks don't conflict
 with one another,
 * no matter what purpose they are held for (eg, session and transaction locks do
not conflict).
 * Nor do the locks of one process in a lock group conflict with those of another
 process in the same group.
 *  So, we must subtract off these locks when determining whether the requested
new lock conflicts with those already held. */
int
LockCheckConflicts(LockMethod lockMethodTable, LOCKMODE lockmode, LOCK *lock,
PROCLOCK *proclock)
{...//conflictMask，是所申请的锁粒度lockmode在相容性锁表lockMethodTable-
>conflictTab[lockmode]里的掩码
    if (!(conflictMask & lock->grantMask)) //与已经被授予的锁粒度（锁类型）不冲突，
    则可以授予。注意
    {
        PROCLOCK_PRINT("LockCheckConflicts: no conflict", proclock);
        return STATUS_OK;
    }
    //有冲突，则检查是不是本进程或本组并发进程持有冲突的锁，如果是，则是一个事务，允许加锁
    myLocks = proclock->holdMask;
    for (i = 1; i <= numLockModes; i++) //遍历每种类型的锁
    {
        if ((conflictMask & LOCKBIT_ON(i)) == 0) //第i位为0，会与任何锁冲突，申请的锁
                                                   就不会被授予，所以计数器值为0
        {
            conflictsRemaining[i] = 0;
            continue;
        }
        conflictsRemaining[i] = lock->granted[i]; //第i位为1，即存在被授予锁的可能性，
                                                   从已经授予的锁找出授予次数
        if (myLocks & LOCKBIT_ON(i)) //第i位为1，且就是所申请的锁，则没有冲突
            --conflictsRemaining[i];
        totalConflictsRemaining += conflictsRemaining[i]; //汇总所有的冲突次数
    }
    /* If no conflicts remain, we get the lock. */
    if (totalConflictsRemaining == 0)  //没有冲突
    {...
        return STATUS_OK;
    }
    /* If no group locking, it's definitely a conflict. */ //冲突总数不是零，且不是并
                                                       行的会话进程组，则冲突一定存在
    if (proclock->groupLeader == MyProc && MyProc->lockGroupLeader == NULL)
    {...
        return STATUS_FOUND;
    }
    /* Locks held in conflicting modes by members of our own lock group are not
     real conflicts;
     * we can subtract those out and see if we still have a conflict.
     * This is O(N) in the number of processes holding or awaiting locks on this object.
     *  We could improve that by making the shared memory state more complex (and
        larger) but it doesn't seem worth it.  */
    procLocks = &(lock->procLocks); //遍历申请lock这个锁的所有会话进程组。注意包括了存在
                                     并行的会话进程组的情况
    otherproclock = (PROCLOCK *) SHMQueueNext(procLocks, procLocks,
    offsetof(PROCLOCK, lockLink));
    while (otherproclock != NULL)
    {
        if (proclock != otherproclock &&
            proclock->groupLeader == otherproclock->groupLeader &&
             //并行的会话进程组的情况
            (otherproclock->holdMask & conflictMask) != 0)
            //并行的会话进程组持有的锁与冲突掩码相容
        {
            int    intersectMask = otherproclock->holdMask & conflictMask;
            //得到与并行的会话进程组持有的锁与冲突掩码相容的锁粒度
            for (i = 1; i <= numLockModes; i++)
            {
                if ((intersectMask & LOCKBIT_ON(i)) != 0) //相容
                {
                    if (conflictsRemaining[i] <= 0)
                        elog(PANIC, "proclocks held do not match lock");
                    conflictsRemaining[i]--;
                    totalConflictsRemaining--;
                }
            }
            if (totalConflictsRemaining == 0)  //没有不相容的，锁可以授予
            {
                PROCLOCK_PRINT("LockCheckConflicts: resolved (group)", proclock);
                return STATUS_OK;
            }
        }
        otherproclock = (PROCLOCK *) SHMQueueNext(procLocks, &otherproclock->
        lockLink, offsetof(PROCLOCK, lockLink));
    }
    /* Nope, it's a real conflict. */
    PROCLOCK_PRINT("LockCheckConflicts: conflicting (group)", proclock);
    return STATUS_FOUND;  //存在冲突
}


```