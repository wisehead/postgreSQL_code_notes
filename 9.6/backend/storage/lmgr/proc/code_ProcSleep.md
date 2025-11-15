#1.ProcSleep

```cpp
//ProcSleep -- put a process to sleep on the specified lock
int
ProcSleep(LOCALLOCK *locallock, LockMethod lockMethodTable)
{...
    if (myHeldLocks != 0) //本等待进程持有其他的锁
    {...
        proc = (PGPROC *) waitQueue->links.next; //欲申请锁的等待队列
        for (i = 0; i < waitQueue->size; i++)    //遍历欲申请锁的等待队列
        {...
            /* Must he wait for me? */
            if (lockMethodTable->conflictTab[proc->waitLockMode] & myHeldLocks)
            //有其他进程在等待我自己（myHeldLocks）
            {
                /* Must I wait for him ? */
                if (lockMethodTable->conflictTab[lockmode] & proc->heldLocks)
                //我自己在等待其他进程持有的某种锁（proc->heldLocks）
                {...//发现有AB-BA式死锁，记录下来，后面处理
                    RememberSimpleDeadLock(MyProc, lockmode, lock, proc);
                    early_deadlock = true;
                    break;
                }
                /* I must go before this waiter.  Check special case. */
                if ((lockMethodTable->conflictTab[lockmode] & aheadRequests) == 0 &&
                //与等待的进程申请的锁不冲突，则可跳出
                    LockCheckConflicts(lockMethodTable, lockmode, lock, proclock) == STATUS_OK)
                    //注意一个个按序遍历等待本进程的其他进程的申请锁的状态，排在前面的进程如
                      不与新申请的锁冲突，则可把新申请锁的这种情况优先授予锁
                {
                    /* Skip the wait and just grant myself the lock. */
                    GrantLock(lock, proclock, lockmode);
                    GrantAwaitedLock();
                    return STATUS_OK;
                }
                /* Break out of loop to put myself before him */
                break; //注意：这里的前提是“有其他进程在等待我自己（myHeldLocks）”
            }
...
        }
    }
    else  //本等待进程不持任何有其他的锁
    {
        /* I hold no locks, so I can't push in front of anyone. */
        proc = (PGPROC *) &(waitQueue->links);
    }
    //Insert self into queue, ahead of the given proc (or at tail of queue).
    SHMQueueInsertBefore(&(proc->links), &(MyProc->links));
    waitQueue->size++;
    lock->waitMask |= LOCKBIT_ON(lockmode); //不能获得申请的锁，则只能处于等待
...
    MyProc->waitStatus = STATUS_WAITING;    //设置处于等待状态
    if (early_deadlock)  //处理之前发现的死锁
    {
        RemoveFromWaitQueue(MyProc, hashcode);
        return STATUS_ERROR;
    }
...
    lockAwaited = locallock; //“lockAwaited”是处于等待其他锁状态的一个重要标志
...
    do
    {
        if (InHotStandby)
        {
            /* Set a timer and wait for that or for the Lock to be granted */
            ResolveRecoveryConflictWithLock(locallock->tag.lock);
        }
        else
        {...
            if (got_deadlock_timeout)
            {
                CheckDeadLock();  //进行死锁检测
                got_deadlock_timeout = false;
            }
            CHECK_FOR_INTERRUPTS();
        }
        myWaitStatus = *((volatile int *) &MyProc->waitStatus);
        //在每轮循环中，保存等待状态（可能异步地被其他进程唤醒）
        if (deadlock_state == DS_BLOCKED_BY_AUTOVACUUM && allow_autovacuum_cancel)
        //没有发生死锁，只是被AUTOVACUUM阻塞
        {...
            if ((autovac_pgxact->vacuumFlags & PROC_IS_AUTOVACUUM) &&
                !(autovac_pgxact->vacuumFlags & PROC_VACUUM_FOR_WRAPAROUND))
            {
                int pid = autovac->pid;    //获取AUTOVACUUM的进程ID
...
                if (kill(pid, SIGINT) < 0)  //发信号给AUTOVACUUM的进程，让其停止
                {...}
...
            }
            else
...
        }
        if (log_lock_waits && deadlock_state != DS_NOT_YET_CHECKED)
        //否则，处理各种死锁的情况，方式是报告错误
        {...
            if (deadlock_state == DS_SOFT_DEADLOCK)
                ereport(LOG,...);
            else if (deadlock_state == DS_HARD_DEADLOCK)
            {    ereport(LOG,...);   }
...
        }
    } while (myWaitStatus == STATUS_WAITING);
      //只要处于等待状态，就循环，进行死锁检测等如上检查
...
    if (MyProc->waitStatus == STATUS_OK)       //获取了锁
        GrantAwaitedLock();
...
}
```