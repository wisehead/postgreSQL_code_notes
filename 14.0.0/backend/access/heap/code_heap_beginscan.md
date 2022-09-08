#1.heap_beginscan

```
heap_beginscan
--RelationIncrementReferenceCount(relation);
--scan = (HeapScanDesc) palloc(sizeof(HeapScanDescData));
--scan->rs_base.rs_key = (ScanKey) palloc(sizeof(ScanKeyData) * nkeys);
--initscan(scan, key, false);
```