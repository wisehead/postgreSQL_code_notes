#1.IndexNext

```
IndexNext
--if (scandesc == NULL)
----index_beginscan
----index_rescan(scandesc,
						 node->iss_ScanKeys, node->iss_NumScanKeys,
						 node->iss_OrderByKeys, node->iss_NumOrderByKeys);
--while (index_getnext_slot(scandesc, direction, slot))
----return slot;
```