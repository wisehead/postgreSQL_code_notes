#1.CreateEntryPages

```
CreateEntryPages
--buildstate->sortstate = tuplesort_begin_heap
--buildstate->reltuples = table_index_build_scan
--tuplesort_performsort(buildstate->sortstate);
--InsertTuples(buildstate->index, buildstate, forkNum);
--tuplesort_end(buildstate->sortstate);
```