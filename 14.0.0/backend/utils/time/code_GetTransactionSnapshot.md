#1.GetTransactionSnapshot

```
GetTransactionSnapshot
--if (HistoricSnapshotActive())
----return HistoricSnapshot;
--if (!FirstSnapshotSet)
----InvalidateCatalogSnapshot
----if (IsolationUsesXactSnapshot())
------if (IsolationIsSerializable())
--------CurrentSnapshot = 	GetSerializableTransactionSnapshot(&CurrentSnapshotData);
------else
--------CurrentSnapshot = GetSnapshotData(&CurrentSnapshotData);
------CurrentSnapshot = CopySnapshot(CurrentSnapshot);                     
------FirstXactSnapshot = CurrentSnapshot;                                 
------/* Mark it as "registered" in FirstXactSnapshot */                   
------FirstXactSnapshot->regd_count++;                                     
------pairingheap_add(&RegisteredSnapshots, &FirstXactSnapshot->ph_node); 
----else
------CurrentSnapshot = GetSnapshotData(&CurrentSnapshotData); 
----FirstSnapshotSet = true;
----return CurrentSnapshot;
--if (IsolationUsesXactSnapshot())
----return CurrentSnapshot;
--InvalidateCatalogSnapshot();
--CurrentSnapshot = GetSnapshotData(&CurrentSnapshotData);
--return CurrentSnapshot;
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