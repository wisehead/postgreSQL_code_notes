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
--TupleDescInitEntry(buildstate->tupdesc, (AttrNumber) 1, "list", INT4OID, -1, 0);
--TupleDescInitEntry(buildstate->tupdesc, (AttrNumber) 2, "tid", TIDOID, -1, 0)
--TupleDescInitEntry(buildstate->tupdesc, (AttrNumber) 3, "vector", RelationGetDescr(index)->attrs[0].atttypid, -1, 0);
--buildstate->slot = MakeSingleTupleTableSlot(buildstate->tupdesc, &TTSOpsVirtual);
--buildstate->centers = VectorArrayInit(buildstate->lists, buildstate->dimensions);
-- buildstate->normvec = InitVector(buildstate->dimensions);
```

#2.caller

- BuildIndex