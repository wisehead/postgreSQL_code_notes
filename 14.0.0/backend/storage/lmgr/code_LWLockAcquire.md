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
----if (!mustwait)
------LOG_LWDEBUG("LWLockAcquire", lock, "acquired, undoing queue");
------LWLockDequeueSelf(lock);
------break;
----LWLockReportWaitStart(lock);
------pgstat_report_wait_start(PG_WAIT_LWLOCK | lock->tranche);
--------*(volatile uint32 *) my_wait_event_info = wait_event_info;
----if (TRACE_POSTGRESQL_LWLOCK_WAIT_START_ENABLED())
			TRACE_POSTGRESQL_LWLOCK_WAIT_START(T_NAME(lock), mode);
----for (;;)
			PGSemaphoreLock(proc->sem);
			if (!proc->lwWaiting)
				break;
			extraWaits++;
----}//end for

		/* Retrying, allow LWLockRelease to release waiters again. */
		pg_atomic_fetch_or_u32(&lock->state, LW_FLAG_RELEASE_OK);
--//end for
```