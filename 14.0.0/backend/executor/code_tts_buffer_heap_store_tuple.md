#1.tts_buffer_heap_store_tuple

```
tts_buffer_heap_store_tuple
--BufferHeapTupleTableSlot *bslot = (BufferHeapTupleTableSlot *) slot;
--if (TTS_SHOULDFREE(slot))
----heap_freetuple(bslot->base.tuple);
```