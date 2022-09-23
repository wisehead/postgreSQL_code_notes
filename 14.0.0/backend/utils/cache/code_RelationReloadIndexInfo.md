#1.RelationReloadIndexInfo

```
RelationReloadIndexInfo
--RelationCloseSmgr
----smgrclose((relation)->rd_smgr);
--if (relation->rd_amcache)
----pfree(relation->rd_amcache);
--ScanPgRelation
--relp = (Form_pg_class) GETSTRUCT(pg_class_tuple);
```