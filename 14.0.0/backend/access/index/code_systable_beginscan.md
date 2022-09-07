#1.systable_beginscan

```
systable_beginscan
--index_open
--table_slot_create
--if (snapshot == NULL)
----relid = RelationGetRelid(heapRelation);
----snapshot = RegisterSnapshot(GetCatalogSnapshot(relid));
------RegisterSnapshotOnOwner
--------ResourceOwnerEnlargeSnapshots(owner);
--------snap->regd_count++;
--------ResourceOwnerRememberSnapshot(owner, snap);
----------ResourceArrayAdd
--sysscan->iscan = index_beginscan(heapRelation, indexRelation,
									 snapshot, nkeys, 0);
									 
--index_rescan(sysscan->iscan, key, nkeys, NULL, 0);
```