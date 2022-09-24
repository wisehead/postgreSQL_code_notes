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
```