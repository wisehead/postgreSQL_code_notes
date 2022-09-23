#1.RelationReloadNailed

```
RelationReloadNailed
--RelationInitPhysicalAddr(relation);
--if (relation->rd_rel->relkind == RELKIND_INDEX)
----RelationReloadIndexInfo
--else
----if (criticalRelcachesBuilt)
------pg_class_tuple = ScanPgRelation(
------relp = (Form_pg_class) GETSTRUCT(pg_class_tuple);
------memcpy(relation->rd_rel, relp, CLASS_TUPLE_SIZE);
------heap_freetuple(pg_class_tuple);
```