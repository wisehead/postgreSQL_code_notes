#1.ivfflatrescan

```
ivfflatrescan
--IvfflatScanOpaque so = (IvfflatScanOpaque) scan->opaque;
--if (!so->first)
----tuplesort_reset(so->sortstate);
--pairingheap_reset(so->listQueue);
--if (keys && scan->numberOfKeys > 0)
----memmove(scan->keyData, keys, scan->numberOfKeys * sizeof(ScanKeyData));
--if (orderbys && scan->numberOfOrderBys > 0)
----memmove(scan->orderByData, orderbys, scan->numberOfOrderBys * sizeof(ScanKeyData));

```