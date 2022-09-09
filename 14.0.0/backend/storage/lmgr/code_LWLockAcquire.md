#1.LWLockAcquire

```
LWLockAcquire
--if (num_held_lwlocks >= MAX_SIMUL_LWLOCKS)
		elog(ERROR, "too many LWLocks taken");
--HOLD_INTERRUPTS();
----InterruptHoldoffCount++)
--for (;;)
----mustwait = LWLockAttemptLock(lock, mode);
----if (!mustwait)
		{
			LOG_LWDEBUG("LWLockAcquire", lock, "immediately acquired lock");
			break;				/* got the lock */
		}
----LWLockQueueSelf(lock, mode);
----mustwait = LWLockAttemptLock(lock, mode);
--//end for
```