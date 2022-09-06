#1.pg_malloc_internal

```cpp
pg_malloc_internal
--malloc
```

#2.flags

```cpp
/*
 * Flags for pg_malloc_extended and palloc_extended, deliberately named
 * the same as the backend flags.
 */
#define MCXT_ALLOC_HUGE         0x01    /* allow huge allocation (> 1 GB) not
                                         * actually used for frontends */
#define MCXT_ALLOC_NO_OOM       0x02    /* no failure if out-of-memory */
#define MCXT_ALLOC_ZERO         0x04    /* zero allocated memory */
```

#3.caller
- pg_malloc
- pg_malloc0
- pg_malloc_extended
- palloc
- palloc0
- palloc_extended