#1.union SemTPadded

```cpp
typedef union SemTPadded
{
	sem_t		pgsem;
	char		pad[PG_CACHE_LINE_SIZE];
} SemTPadded;
```