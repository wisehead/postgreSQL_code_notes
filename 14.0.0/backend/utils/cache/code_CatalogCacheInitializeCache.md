#1.CatalogCacheInitializeCache

```cpp
CatalogCacheInitializeCache
--relation = table_open(cache->cc_reloid, AccessShareLock);
----relation_open
--tupdesc = CreateTupleDescCopyConstr(RelationGetDescr(relation));
----CreateTemplateTupleDesc
----memcpy(TupleDescAttr(desc, 0),
		   TupleDescAttr(tupdesc, 0),
		   desc->natts * sizeof(FormData_pg_attribute));
--table_close(relation, AccessShareLock);
--for (i = 0; i < cache->cc_nkeys; ++i)
----GetCCHashEqFuncs(keytype,
						 &cache->cc_hashfunc[i],
						 &eqfunc,
						 &cache->cc_fastequal[i]);
----fmgr_info_cxt(eqfunc,
					  &cache->cc_skey[i].sk_func,
					  CacheMemoryContext);

```