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

```