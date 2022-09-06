#1.SearchCatCacheInternal

```
SearchCatCacheInternal
--if (unlikely(cache->cc_tupdesc == NULL))
----CatalogCacheInitializeCache(cache);
```