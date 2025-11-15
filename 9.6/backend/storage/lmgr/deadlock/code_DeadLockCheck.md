#1.DeadLockCheck

```cpp
/*
 * DeadLockCheck -- Checks for deadlocks for a given process
 *
 * This code looks for deadlocks involving the given process.  If any are found,
   it tries to rearrange lock wait queues to resolve the
 * deadlock.  If resolution is impossible, return DS_HARD_DEADLOCK --- the caller
    is then expected to abort the given proc's transaction.
*/
DeadLockState
DeadLockCheck(PGPROC *proc) //检查指定会话进程proc中是否存在死锁
{...
    /* Search for deadlocks and possible fixes */
    if (DeadLockCheckRecurse(proc))  //递归地进行死锁检测，如果存在soft edge，
    则可以消除其对应的死锁情况
    {...
        if (!FindLockCycle(proc, possibleConstraints, &nSoftEdges))
        //发生死锁，但又不存在环，数据库引擎的内存结构可能被异常破坏了
            elog(FATAL, "deadlock seems to have disappeared");
        //只好报告FATAL错误，强制数据库引擎系统退出
        return DS_HARD_DEADLOCK;    /* cannot find a non-deadlocked state */
    }
    //接下来，肯定是存在soft edge的情况
    for (i = 0; i < nWaitOrders; i++)/* Apply any needed rearrangements of wait queues */
     //遍历修复后等待队列
    {
        LOCK       *lock = waitOrders[i].lock;
        //被DeadLockCheckRecurse(proc)修复后的等待队列的顺序
        PGPROC     **procs = waitOrders[i].procs;
        int        nProcs = waitOrders[i].nProcs;
        PROC_QUEUE *waitQueue = &(lock->waitProcs);
...
        /* Reset the queue and re-add procs in the desired order */
        ProcQueueInit(waitQueue);
        for (j = 0; j < nProcs; j++)  //重新排定等待队列
        {
            SHMQueueInsertBefore(&(waitQueue->links), &(procs[j]->links));
            waitQueue->size++;
        }
...
        /* See if any waiters for the lock can be woken up now */
        ProcLockWakeup(GetLocksMethodTable(lock), lock);
        //给予等待队列中的处于等待状态的进程运行的机会
    }
    /* Return code tells caller if we had to escape a deadlock or not */
    if (nWaitOrders > 0)
        return DS_SOFT_DEADLOCK;
    else if (blocking_autovacuum_proc != NULL)
        return DS_BLOCKED_BY_AUTOVACUUM;
    else
        return DS_NO_DEADLOCK;
}

```