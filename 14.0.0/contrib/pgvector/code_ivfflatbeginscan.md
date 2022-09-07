#1.ivfflatbeginscan

```
ivfflatbeginscan
--RelationGetIndexScan
--IvfflatGetLists
--so = (IvfflatScanOpaque) palloc(offsetof(IvfflatScanOpaqueData, lists) + probes * sizeof(IvfflatScanList));
--so->procinfo = index_getprocinfo(index, 1, IVFFLAT_DISTANCE_PROC);
--so->normprocinfo = IvfflatOptionalProcInfo(index, IVFFLAT_NORM_PROC);
--so->collation = index->rd_indcollation[0];
--so->tupdesc = CreateTemplateTupleDesc(3);
--TupleDescInitEntry(so->tupdesc, (AttrNumber) 1, "distance", FLOAT8OID, -1, 0);
--TupleDescInitEntry(so->tupdesc, (AttrNumber) 2, "tid", TIDOID, -1, 0);
--TupleDescInitEntry(so->tupdesc, (AttrNumber) 3, "indexblkno", INT4OID, -1, 0);
--so->sortstate = tuplesort_begin_heap(so->tupdesc, 1, attNums, sortOperators, sortCollations, nullsFirstFlags, work_mem, NULL, false);
```