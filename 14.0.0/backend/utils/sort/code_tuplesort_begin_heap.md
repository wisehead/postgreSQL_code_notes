#1.tuplesort_begin_heap

```
tuplesort_begin_heap
--tuplesort_begin_common
--oldcontext = MemoryContextSwitchTo(state->maincontext);
--state->sortKeys = (SortSupport) palloc0(nkeys * sizeof(SortSupportData));
--for (i = 0; i < nkeys; i++)
----SortSupport sortKey = state->sortKeys + i;
----sortKey->ssup_cxt = CurrentMemoryContext;
		sortKey->ssup_collation = sortCollations[i];
		sortKey->ssup_nulls_first = nullsFirstFlags[i];
		sortKey->ssup_attno = attNums[i];
----PrepareSortSupportFromOrderingOp
--MemoryContextSwitchTo(oldcontext);	
```