#1.SearchCatCacheMiss

```
SearchCatCacheMiss
--relation = table_open(cache->cc_reloid, AccessShareLock);
--scandesc = systable_beginscan(relation,
								  cache->cc_indexoid,
								  IndexScanOK(cache, cur_skey),
								  NULL,
								  nkeys,
								  cur_skey);
--while (HeapTupleIsValid(ntp = systable_getnext(scandesc)))
----ct = CatalogCacheCreateEntry(cache, ntp, arguments,
									 hashValue, hashIndex,
									 false);
----ct->refcount++;
----break;					/* assume only one match */
--systable_endscan(scandesc);
--table_close(relation, AccessShareLock);
--return &ct->tuple;
```