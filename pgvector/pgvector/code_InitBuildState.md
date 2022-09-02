#1.InitBuildState

```cpp
InitBuildState
--IvfflatGetLists
----*opts = (IvfflatOptions *) index->rd_options;
--buildstate->dimensions = TupleDescAttr(index->rd_att, 0)->atttypmod;
--buildstate->procinfo = index_getprocinfo(index, 1, IVFFLAT_DISTANCE_PROC);
--buildstate->normprocinfo = IvfflatOptionalProcInfo(index, IVFFLAT_NORM_PROC);
--buildstate->kmeansnormprocinfo = IvfflatOptionalProcInfo(index, IVFFLAT_KMEANS_NORM_PROC);
--buildstate->tupdesc = CreateTemplateTupleDesc(3);
```

#2.caller

- BuildIndex