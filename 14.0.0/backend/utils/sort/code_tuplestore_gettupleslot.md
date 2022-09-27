#1.tuplestore_gettupleslot

```
tuplestore_gettupleslot
--tuple = (MinimalTuple) tuplestore_gettuple(state, forward, &should_free);
--if (tuple)
----if (copy && !should_free)
------tuple = heap_copy_minimal_tuple(tuple);
------should_free = true;
----ExecStoreMinimalTuple(tuple, slot, should_free);
------tts_minimal_store_tuple
----return true;
--else
----ExecClearTuple(slot);
----return false;
```