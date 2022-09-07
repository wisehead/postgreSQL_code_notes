#1.CatCacheRemoveCList

```
CatCacheRemoveCList
--for (i = cl->n_members; --i >= 0;)
----CatCTup    *ct = cl->members[i];
----ct->c_list = NULL;
----if (ct->refcount == 0)
------CatCacheRemoveCTup
--dlist_delete(&cl->cache_elem);
--CatCacheFreeKeys

```

#2. CatCacheRemoveCTup

```
CatCacheRemoveCTup
--if (ct->c_list)
----CatCacheRemoveCList
--dlist_delete(&ct->cache_elem);
--if (ct->negative)
----CatCacheFreeKeys(cache->cc_tupdesc, cache->cc_nkeys,
						 cache->cc_keyno, ct->keys);
--pfree(ct);						 
```