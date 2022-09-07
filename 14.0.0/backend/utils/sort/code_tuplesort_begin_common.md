#1.tuplesort_begin_common

```
tuplesort_begin_common
--maincontext = AllocSetContextCreate(CurrentMemoryContext,
										"TupleSort main",
										ALLOCSET_DEFAULT_SIZES);

----AllocSetContextCreateInternal
--sortcontext = AllocSetContextCreate(maincontext,
										"TupleSort sort",
										ALLOCSET_DEFAULT_SIZES);
--oldcontext = MemoryContextSwitchTo(maincontext);
--state = (Tuplesortstate *) palloc0(sizeof(Tuplesortstate));
--tuplesort_begin_batch(state);
----state->tuplecontext = AllocSetContextCreate(state->sortcontext,
												"Caller tuples",
												ALLOCSET_DEFAULT_SIZES);
----state->memtuples = (SortTuple *) palloc(state->memtupsize * sizeof(SortTuple));							
--MemoryContextSwitchTo(oldcontext);

```