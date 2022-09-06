#1.ComputeCenters

```cpp
ComputeCenters
--VectorArrayInit
----palloc0
------pg_malloc_internal
--SampleRows
--UpdateProgress
----pgstat_progress_update_param
--IvfflatKmeans
/* Free samples before we allocate more memory */
--pfree(buildstate->samples);
```