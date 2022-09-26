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
/* Free samples before we allocate more memory */
--pfree(buildstate->samples);
```

#2. IvfflatKmeans

```
IvfflatKmeans(buildstate->index, buildstate->samples, buildstate->centers)  
--if (samples->length <= centers->maxlen)                                   
----QuickCenters(index, samples, centers);                                  
--else                                                                      
----ElkanKmeans(index, samples, centers);                                   
--CheckCenters                                                              

```

#3.CheckCenters
```
CheckCenters
--qsort(centers->items, centers->length, VECTOR_SIZE(centers->dim), CompareVectors);
--for (i = 1; i < centers->length; i++)
----if (CompareVectors(VectorArrayGet(centers, i), VectorArrayGet(centers, i - 1)) == 0)
------elog(ERROR, "Duplicate centers detected. Please report a bug.");
--normprocinfo = IvfflatOptionalProcInfo(index, IVFFLAT_NORM_PROC);
--for (i = 0; i < centers->length; i++)
----norm = DatumGetFloat8(FunctionCall1Coll(normprocinfo, collation, 		PointerGetDatum(VectorArrayGet(centers, i))));
----if (norm == 0)
------elog(ERROR, "Zero norm detected. Please report a bug.");
```