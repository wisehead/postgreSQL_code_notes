#1.relation_open

```cpp
relation_open
--LockRelationOid(relationId, lockmode);
--RelationIdGetRelation
--pgstat_initstats(r);
----rel->pgstat_info = get_tabstat_entry(rel_id, rel->rd_rel->relisshared);
------hash_entry = hash_search(pgStatTabHash, &rel_id, HASH_ENTER, &found);
```

#2.RelationIdGetRelation

```cpp
RelationIdGetRelation
--RelationIdCacheLookup(relationId, rd);
----hentry = (RelIdCacheEnt *) hash_search(RelationIdCache, \
										   (void *) &(ID), \
										   HASH_FIND, NULL); \
--if (RelationIsValid(rd))
----RelationIncrementReferenceCount(rd);
------ResourceOwnerEnlargeRelationRefs
--------ResourceArrayEnlarge//预分配，预留资源
------rel->rd_refcnt += 1;
------ResourceOwnerRememberRelationRef
--------ResourceArrayAdd
----if (!rd->rd_isvalid)
------if (rd->rd_rel->relkind == RELKIND_INDEX ||
				rd->rd_rel->relkind == RELKIND_PARTITIONED_INDEX)
--------RelationReloadIndexInfo(rd);
------else
--------RelationClearRelation(rd, true);
----return rd;
--rd = RelationBuildDesc(relationId, true);					   
```