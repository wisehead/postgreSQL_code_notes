#1. heapam_index_build_range_scan

```
heapam_index_build_range_scan
--is_system_catalog = IsSystemRelation(heapRelation);
--estate = CreateExecutorState();
--slot = table_slot_create(heapRelation, NULL);
--econtext->ecxt_scantuple = slot;
--predicate = ExecPrepareQual(indexInfo->ii_Predicate, estate);
--if (!scan)
----scan = table_beginscan_strat
--else
----snapshot = scan->rs_snapshot;
--hscan = (HeapScanDesc) scan;
--if (progress)
----if (hscan->rs_base.rs_parallel != NULL)
------pbscan = (ParallelBlockTableScanDesc) hscan->rs_base.rs_parallel;
------nblocks = pbscan->phs_nblocks;
----else
------nblocks = hscan->rs_nblocks;
--while ((heapTuple = heap_getnext(scan, ForwardScanDirection)) != NULL)
----if (hscan->rs_cblock != root_blkno)
------Page		page = BufferGetPage(hscan->rs_cbuf);
------LockBuffer(hscan->rs_cbuf, BUFFER_LOCK_SHARE); 
------heap_get_root_tuples(page, root_offsets);      
------LockBuffer(hscan->rs_cbuf, BUFFER_LOCK_UNLOCK);                                             
------root_blkno = hscan->rs_cblock;                 
----if (snapshot == SnapshotAny)
------switch (HeapTupleSatisfiesVacuum(heapTuple, OldestXmin,
											 hscan->rs_cbuf))
------//todo
----else
------tupleIsAlive = true;
------reltuples += 1;						
----ExecStoreBufferHeapTuple(heapTuple, slot, hscan->rs_cbuf);
------tts_buffer_heap_store_tuple(slot, tuple, buffer, false);
----ExecQual(predicate, econtext))
----FormIndexDatum
```