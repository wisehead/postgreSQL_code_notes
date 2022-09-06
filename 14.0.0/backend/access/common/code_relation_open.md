#1.relation_open

```cpp
relation_open
--LockRelationOid(relationId, lockmode);
--RelationIdGetRelation
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
--else	//(RelationIsValid(rd))
----									   
```