#1.LWLockAcquire

```
LWLockAcquire
--if (num_held_lwlocks >= MAX_SIMUL_LWLOCKS)
		elog(ERROR, "too many LWLocks taken");
--HOLD_INTERRUPTS();
----InterruptHoldoffCount++)
--for (;;)
----mustwait = LWLockAttemptLock(lock, mode);
--//end for
```