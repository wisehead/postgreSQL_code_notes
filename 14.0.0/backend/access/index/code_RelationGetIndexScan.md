#1.RelationGetIndexScan

```
RelationGetIndexScan
--scan = (IndexScanDesc) palloc(sizeof(IndexScanDescData));
--scan->keyData = (ScanKey) palloc(sizeof(ScanKeyData) * nkeys);
--scan->orderByData = (ScanKey) palloc(sizeof(ScanKeyData) * norderbys);
```