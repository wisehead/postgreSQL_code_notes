#1.PGSemaphoreLock

```
PGSemaphoreLock
--do
	{
		errStatus = sem_wait(PG_SEM_REF(sema));
	} while (errStatus < 0 && errno == EINTR);
```