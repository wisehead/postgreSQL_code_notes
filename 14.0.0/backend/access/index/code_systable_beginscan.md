#1.systable_beginscan

```
systable_beginscan
--index_open
--table_slot_create
--if (snapshot == NULL)
----relid = RelationGetRelid(heapRelation);
----snapshot = RegisterSnapshot(GetCatalogSnapshot(relid));
```