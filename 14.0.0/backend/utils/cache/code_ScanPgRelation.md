#1.ScanPgRelation

```cpp
ScanPgRelation
--ScanKeyInit(&key[0],
				Anum_pg_class_oid,
				BTEqualStrategyNumber, F_OIDEQ,
				ObjectIdGetDatum(targetRelId));

--pg_class_desc = table_open(RelationRelationId, AccessShareLock);
--if (force_non_historic)
----snapshot = GetNonHistoricCatalogSnapshot(RelationRelationId);
------InvalidateCatalogSnapshot
--------pairingheap_remove(&RegisteredSnapshots, &CatalogSnapshot->ph_node);
------GetSnapshotData
------pairingheap_add(&RegisteredSnapshots, &CatalogSnapshot->ph_node);
--systable_beginscan
--systable_getnext

```