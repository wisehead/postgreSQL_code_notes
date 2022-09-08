#1.readtup_heap

```
readtup_heap
--tuple = (MinimalTuple) readtup_alloc(state, tuplen);
--LogicalTapeReadExact(state->tapeset, tapenum,
						 tupbody, tupbodylen);
----LogicalTapeRead
--if (state->randomAccess)
----LogicalTapeReadExact(state->tapeset, tapenum,
							 &tuplen, sizeof(tuplen));
--stup->tuple = (void *) tuple;
--htup.t_len = tuple->t_len + MINIMAL_TUPLE_OFFSET;
--htup.t_data = (HeapTupleHeader) ((char *) tuple - MINIMAL_TUPLE_OFFSET);
--stup->datum1 = heap_getattr(&htup,
								state->sortKeys[0].ssup_attno,
								state->tupDesc,
								&stup->isnull1);						 
```