#1.struct IndexBuildResult

```cpp
/*
 * Struct for statistics returned by ambuild
 */
typedef struct IndexBuildResult
{
    double      heap_tuples;    /* # of tuples seen in parent table */
    double      index_tuples;   /* # of tuples inserted into index */
} IndexBuildResult;
```