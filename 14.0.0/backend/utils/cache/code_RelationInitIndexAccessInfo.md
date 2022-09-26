#1.RelationInitIndexAccessInfo

```
RelationInitIndexAccessInfo
--SearchSysCache1(INDEXRELID,
							ObjectIdGetDatum(RelationGetRelid(relation)));
--relation->rd_indextuple = heap_copytuple(tuple);
--relation->rd_index = (Form_pg_index) GETSTRUCT(relation->rd_indextuple);        
--SearchSysCache1(AMOID, ObjectIdGetDatum(relation->rd_rel->relam));
--aform = (Form_pg_am) GETSTRUCT(tuple);
--relation->rd_amhandler = aform->amhandler;
--InitIndexAmRoutine
--IndexSupportInitialize
----LookupOpclassInfo
--RelationGetIndexAttOptions
----for (i = 0; i < natts; i++)
------Datum		attoptions = get_attoptions(relid, i + 1);
--------SearchSysCache2
--------SysCacheGetAttr
------opts[i] = index_opclass_options(relation, i + 1, attoptions, false);
--relation->rd_opcoptions = CopyIndexAttOptions(opts, natts);
```