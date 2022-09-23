#1.tts_buffer_heap_store_tuple

```
tts_buffer_heap_store_tuple
--BufferHeapTupleTableSlot *bslot = (BufferHeapTupleTableSlot *) slot;
--if (TTS_SHOULDFREE(slot))
----heap_freetuple(bslot->base.tuple);
--slot->tts_flags &= ~TTS_FLAG_EMPTY;
	slot->tts_nvalid = 0;
	bslot->base.tuple = tuple;
	bslot->base.off = 0;
	slot->tts_tid = tuple->t_self;
--if (bslot->buffer != buffer)
----if (BufferIsValid(bslot->buffer))
------ReleaseBuffer(bslot->buffer);
----bslot->buffer = buffer;
----if (!transfer_pin && BufferIsValid(buffer))
------IncrBufferRefCount(buffer);
----else if (transfer_pin && BufferIsValid(buffer))
------ReleaseBuffer(buffer);
```