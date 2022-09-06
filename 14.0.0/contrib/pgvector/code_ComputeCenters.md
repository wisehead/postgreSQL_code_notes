#1.ComputeCenters

```cpp
ComputeCenters
--VectorArrayInit
----palloc0
------pg_malloc_internal
--SampleRows
--UpdateProgress(PROGRESS_CREATEIDX_SUBPHASE, PROGRESS_IVFFLAT_PHASE_KMEANS);
----pgstat_progress_update_param
--IvfflatKmeans(buildstate->index, buildstate->samples, buildstate->centers)
----if (samples->length <= centers->maxlen)
------QuickCenters(index, samples, centers);
----else
------ElkanKmeans(index, samples, centers);
----CheckCenters
/* Free samples before we allocate more memory */
--pfree(buildstate->samples);
```