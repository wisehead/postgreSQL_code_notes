#1.tuplestore_gettuple

```
tuplestore_gettuple
--switch (state->status)
----case TSS_INMEM:
------*should_free = false;
------if (forward)
--------if (readptr->eof_reached)
----------return NULL;
--------if (readptr->current < state->memtupcount)
----------return state->memtuples[readptr->current++];
--------readptr->eof_reached = true;
--------return NULL;

```