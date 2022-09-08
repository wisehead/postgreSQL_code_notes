#1.tuplesort_gettuple_common

```
tuplesort_gettuple_common
--switch (state->status)
--case TSS_SORTEDINMEM:
----if (forward)
------if (state->current < state->memtupcount)
--------*stup = state->memtuples[state->current++];
--------return true;
------state->eof_reached = true;
------return false;
----else// if (forward)
------*stup = state->memtuples[state->current - 1];

--case TSS_SORTEDONTAPE:
----if (state->lastReturnedTuple)
------RELEASE_SLAB_SLOT(state, state->lastReturnedTuple);
------state->lastReturnedTuple = NULL;
----if (forward)
------uplen = getlen(state, state->result_tape, true)
--------LogicalTapeRead
------READTUP(state, stup, state->result_tape, tuplen);
--------readtup_heap/readtup_index
------state->lastReturnedTuple = stup->tuple;
------return true;
----//backword
----if (state->eof_reached)
------

```