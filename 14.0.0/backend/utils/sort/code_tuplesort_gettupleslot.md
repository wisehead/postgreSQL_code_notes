#1.tuplesort_gettupleslot

```
tuplesort_gettupleslot
--tuplesort_gettuple_common
--ExecStoreMinimalTuple((MinimalTuple) stup.tuple, slot, copy);
----tts_minimal_store_tuple
------tts_minimal_clear
--------if (TTS_SHOULDFREE(slot))
----------heap_free_minimal_tuple(mslot->mintuple);
------------pfree(mtup);
------mslot->mintuple = mtup;
```