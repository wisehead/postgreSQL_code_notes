#1.RelationInitTableAccessMethod

```
RelationInitTableAccessMethod
--if (relation->rd_rel->relkind == RELKIND_SEQUENCE)
----relation->rd_amhandler = F_HEAP_TABLEAM_HANDLER;
--else if (IsCatalogRelation(relation))
----relation->rd_amhandler = F_HEAP_TABLEAM_HANDLER;
--else
----tuple = SearchSysCache1(AMOID,
								ObjectIdGetDatum(relation->rd_rel->relam));
----aform = (Form_pg_am) GETSTRUCT(tuple);
----relation->rd_amhandler = aform->amhandler;
----ReleaseSysCache(tuple);
--InitTableAmRoutine
----relation->rd_tableam = GetTableAmRoutine(relation->rd_amhandler);

```

#2. GetTableAmRoutine

```
GetTableAmRoutine
--datum = OidFunctionCall0(amhandler);
--routine = (TableAmRoutine *) DatumGetPointer(datum);
--
```