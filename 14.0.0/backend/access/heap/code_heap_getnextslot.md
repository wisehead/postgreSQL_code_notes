#1.heap_getnextslot

```
heap_getnextslot
--if (sscan->rs_flags & SO_ALLOW_PAGEMODE)
----heapgettup_pagemode(scan, direction, sscan->rs_nkeys, sscan->rs_key);
--else
----heapgettup(scan, direction, sscan->rs_nkeys, sscan->rs_key);
--if (scan->rs_ctup.t_data == NULL)
----ExecClearTuple(slot);
------slot->tts_ops->clear(slot);
--------tts_heap_clear
----return false;
--ExecStoreBufferHeapTuple
----tts_buffer_heap_store_tuple
```