#1.GetTransactionSnapshot

```
GetTransactionSnapshot
--if (HistoricSnapshotActive())
----return HistoricSnapshot;
--if (!FirstSnapshotSet)
----InvalidateCatalogSnapshot
```

#2. InvalidateCatalogSnapshot

```
InvalidateCatalogSnapshot
--if (CatalogSnapshot)
----pairingheap_remove(&RegisteredSnapshots, &CatalogSnapshot->ph_node);
----CatalogSnapshot = NULL;
----SnapshotResetXmin();
```

#3.SnapshotResetXmin

```
SnapshotResetXmin
--if (ActiveSnapshot != NULL)
----return;
--if (pairingheap_is_empty(&RegisteredSnapshots))
----MyProc->xmin = InvalidTransactionId;
----return;
--minSnapshot = pairingheap_container(SnapshotData, ph_node,
										pairingheap_first(&RegisteredSnapshots));
--if (TransactionIdPrecedes(MyProc->xmin, minSnapshot->xmin))
----MyProc->xmin = minSnapshot->xmin;										
```