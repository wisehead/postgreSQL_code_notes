#1.systable_endscan

```
systable_endscan
--if (sysscan->slot)
----ExecDropSingleTupleTableSlot(sysscan->slot);
------ExecClearTuple(slot);
------slot->tts_ops->release(slot);
------if (slot->tts_tupleDescriptor)
--------ReleaseTupleDesc(slot->tts_tupleDescriptor);
----sysscan->slot = NULL;
--if (sysscan->irel)
----index_endscan(sysscan->iscan);
----index_close(sysscan->irel, AccessShareLock);
--else
----table_endscan(sysscan->scan);
------scan->rs_rd->rd_tableam->scan_end(scan);
--------heap_endscan
--if (sysscan->snapshot)
----UnregisterSnapshot(sysscan->snapshot);
```