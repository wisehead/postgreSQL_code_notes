#1.RelationReloadIndexInfo

```
RelationReloadIndexInfo
--RelationCloseSmgr
----smgrclose((relation)->rd_smgr);
--if (relation->rd_amcache)
----pfree(relation->rd_amcache);
--ScanPgRelation
--relp = (Form_pg_class) GETSTRUCT(pg_class_tuple);
--memcpy(relation->rd_rel, relp, CLASS_TUPLE_SIZE);
--RelationParseRelOptions(relation, pg_class_tuple);
--heap_freetuple(pg_class_tuple);
--RelationInitPhysicalAddr(relation);
--if (!IsSystemRelation(relation))
----tuple = SearchSysCache1(INDEXRELID,
								ObjectIdGetDatum(RelationGetRelid(relation)));
------SearchCatCache1
--------SearchCatCacheInternal	
----index = (Form_pg_index) GETSTRUCT(tuple);
----ReleaseSysCache(tuple);
```