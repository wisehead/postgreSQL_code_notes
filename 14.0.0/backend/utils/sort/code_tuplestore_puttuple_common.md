#1.tuplestore_puttuple_common

```
tuplestore_puttuple_common
--switch (state->status)
----case TSS_INMEM:
------if (state->memtupcount >= state->memtupsize - 1)
--------(void) grow_memtuples(state);
------state->memtuples[state->memtupcount++] = tuple;
------if (state->memtupcount < state->memtupsize && !LACKMEM(state))
--------return;
------PrepareTempTablespaces();
------state->myfile = BufFileCreateTemp(state->interXact);
------dumptuples(state);
------break;
```