#1.ProcLockWakeup

```cpp
void
ProcLockWakeup(LockMethod lockMethodTable, LOCK *lock)
//唤醒lock对象上的等待队列中的进程
{
    PROC_QUEUE *waitQueue = &(lock->waitProcs);  //获得lock对象上的等待队列
...
    proc = (PGPROC *) waitQueue->links.next;     //获得lock对象上的等待队列中的进程
    while (queue_size-- > 0)                     //遍历lock对象上的等待队列中的进程
    {
        LOCKMODE    lockmode = proc->waitLockMode;
        /*
         * Waken if (a) doesn't conflict with requests of earlier waiters, and
         * (b) doesn't conflict with already-held locks.
         */
        if ((lockMethodTable->conflictTab[lockmode] & aheadRequests) == 0 &&
        //与本进程上等待队列中前面的等待者持有的锁不冲突
            LockCheckConflicts(lockMethodTable, //与本进程上已经持有的锁不冲突
                               lockmode, lock, proc->waitProcLock) == STATUS_OK)
        {
            /* OK to waken */
            GrantLock(lock, proc->waitProcLock, lockmode);
            proc = ProcWakeup(proc, STATUS_OK);
        }
        else
        {
            aheadRequests |= LOCKBIT_ON(lockmode); //记录本进程上等待队列中前面的等待
                                                  者持有的锁，供下个等待者下次判断使用
            proc = (PGPROC *) proc->links.next;
        }
    }
...
}
```