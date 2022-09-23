#1.struct BufferHeapTupleTableSlot

```cpp
/* heap tuple residing in a buffer */
typedef struct BufferHeapTupleTableSlot
{
	HeapTupleTableSlot base;

	/*
	 * If buffer is not InvalidBuffer, then the slot is holding a pin on the
	 * indicated buffer page; drop the pin when we release the slot's
	 * reference to that buffer.  (TTS_FLAG_SHOULDFREE should not be set in
	 * such a case, since presumably tts_tuple is pointing into the buffer.)
	 */
	Buffer		buffer;			/* tuple's buffer, or InvalidBuffer */
} BufferHeapTupleTableSlot;
```