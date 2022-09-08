#1.initscan

```
initscan
--if (scan->rs_base.rs_parallel != NULL)
----bpscan = (ParallelBlockTableScanDesc) scan->rs_base.rs_parallel;
----scan->rs_nblocks = bpscan->phs_nblocks;
--else
----scan->rs_nblocks = RelationGetNumberOfBlocks(scan->rs_base.rs_rd);
--memcpy(scan->rs_base.rs_key, key, scan->rs_base.rs_nkeys * sizeof(ScanKeyData));
--if (scan->rs_base.rs_flags & SO_TYPE_SEQSCAN)
		pgstat_count_heap_scan(scan->rs_base.rs_rd);
```