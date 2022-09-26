#1.SearchCatCacheInternal

```
SearchCatCacheInternal
--if (unlikely(cache->cc_tupdesc == NULL))
----CatalogCacheInitializeCache(cache);
--hashValue = CatalogCacheComputeHashValue(cache, nkeys, v1, v2, v3, v4);
--hashIndex = HASH_INDEX(hashValue, cache->cc_nbuckets);
--bucket = &cache->cc_bucket[hashIndex];
--dlist_foreach(iter, bucket)
----ct = dlist_container(CatCTup, cache_elem, iter.cur);
----dlist_move_head(bucket, &ct->cache_elem);
----if (!ct->negative)
------ct->refcount++;
------return &ct->tuple;
----else
------return NULL;
--return SearchCatCacheMiss(cache, nkeys, hashValue, hashIndex, v1, v2, v3, v4);
```