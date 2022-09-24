#1.index_opclass_options

```
index_opclass_options
--procid = index_getprocid(indrel, attnum, amoptsprocnum);
--if (!OidIsValid(procid))
----indclassDatum = SysCacheGetAttr(INDEXRELID, indrel->rd_indextuple,
										Anum_pg_index_indclass, &isnull);
----indclass = (oidvector *) DatumGetPointer(indclassDatum);
----opclass = indclass->values[attnum - 1];
--init_local_reloptions(&relopts, 0);
--procinfo = index_getprocinfo(indrel, attnum, amoptsprocnum);							
```