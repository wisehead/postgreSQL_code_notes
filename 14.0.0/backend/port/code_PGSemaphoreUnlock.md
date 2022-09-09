#1.PGSemaphoreUnlock

```
PGSemaphoreUnlock
--/*
	 * Note: if errStatus is -1 and errno == EINTR then it means we returned
	 * from the operation prematurely because we were sent a signal.  So we
	 * try and unlock the semaphore again. Not clear this can really happen,
	 * but might as well cope.
	 */
	do
	{
		errStatus = sem_post(PG_SEM_REF(sema));
	} while (errStatus < 0 && errno == EINTR);
```