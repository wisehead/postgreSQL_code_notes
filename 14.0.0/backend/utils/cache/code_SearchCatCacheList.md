#1.SearchCatCacheList

```
SearchCatCacheList
--lHashValue = CatalogCacheComputeHashValue(cache, nkeys, v1, v2, v3, v4);
--dlist_foreach(iter, &cache->cc_lists)
----cl = dlist_container(CatCList, cache_elem, iter.cur);
----CatalogCacheCompareTuple
----dlist_move_head(&cache->cc_lists, &cl->cache_elem);
----ResourceOwnerEnlargeCatCacheListRefs(CurrentResourceOwner);
----cl->refcount++;
----ResourceOwnerRememberCatCacheListRef(CurrentResourceOwner, cl);
//not in cache
--ResourceOwnerEnlargeCatCacheListRefs(CurrentResourceOwner);
--memcpy(cur_skey, cache->cc_skey, sizeof(ScanKeyData) * cache->cc_nkeys);
--relation = table_open(cache->cc_reloid, AccessShareLock);
--systable_beginscan
--while (HeapTupleIsValid(ntp = systable_getnext(scandesc)))
----CatalogCacheComputeTupleHashValue
----hashIndex = HASH_INDEX(hashValue, cache->cc_nbuckets);
----bucket = &cache->cc_bucket[hashIndex];
----dlist_foreach(iter, bucket)
------ct = dlist_container(CatCTup, cache_elem, iter.cur);
----if (!found)
------CatalogCacheCreateEntry
----ctlist = lappend(ctlist, ct);
--systable_endscan(scandesc);
--table_close(relation, AccessShareLock);
--nmembers = list_length(ctlist);
--cl = (CatCList *)
			palloc(offsetof(CatCList, members) + nmembers * sizeof(CatCTup *));
--CatCacheCopyKeys(cache->cc_tupdesc, nkeys, cache->cc_keyno,
						 arguments, cl->keys);
--foreach(ctlist_item, ctlist)
----cl->members[i++] = ct = (CatCTup *) lfirst(ctlist_item);
----ct->c_list = cl;
----ct->refcount--;
--dlist_push_head(&cache->cc_lists, &cl->cache_elem);
--cl->refcount++;
--ResourceOwnerRememberCatCacheListRef(CurrentResourceOwner, cl);
```