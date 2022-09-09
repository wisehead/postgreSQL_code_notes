#1.LWLockQueueSelf

```
LWLockQueueSelf
--LWLockWaitListLock(lock);
--pg_atomic_fetch_or_u32(&lock->state, LW_FLAG_HAS_WAITERS);
--MyProc->lwWaiting = true;
--MyProc->lwWaitMode = mode;
--if (mode == LW_WAIT_UNTIL_FREE)
----proclist_push_head(&lock->waiters, MyProc->pgprocno, lwWaitLink);
--else
----proclist_push_tail(&lock->waiters, MyProc->pgprocno, lwWaitLink);
```