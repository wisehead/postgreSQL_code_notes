#1.struct PGSemaphoreData

```cpp
/* typedef PGSemaphore is equivalent to pointer to sem_t */
typedef struct PGSemaphoreData
{
	SemTPadded	sem_padded;
} PGSemaphoreData;
```