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
----
```