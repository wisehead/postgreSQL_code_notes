#1.RecoveryInProgress

```
RecoveryInProgress
--if (!LocalRecoveryInProgress)
----return false;
--else
----volatile XLogCtlData *xlogctl = XLogCtl;
----LocalRecoveryInProgress = (xlogctl->SharedRecoveryState != RECOVERY_STATE_DONE);
----if (!LocalRecoveryInProgress)
----{
			/*
			 * If we just exited recovery, make sure we read TimeLineID and
			 * RedoRecPtr after SharedRecoveryState (for machines with weak
			 * memory ordering).
			 */
			pg_memory_barrier();
			InitXLOGAccess();
----}
----return LocalRecoveryInProgress;
```