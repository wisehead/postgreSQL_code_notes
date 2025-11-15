#1.ExecGrant_Relation

```cpp
ExecGrant_Relation(InternalGrant *istmt) //需要修改系统表，故在内存中直接对系统表加排它锁
{...
    relation = heap_open(RelationRelationId, RowExclusiveLock);
    // RelationRelationId，值为1259，标识系统表pg_class
    attRelation = heap_open(AttributeRelationId, RowExclusiveLock);
    // AttributeRelationId，值为1249，标识系统表pg_attribute
...
    heap_close(attRelation, RowExclusiveLock);
    heap_close(relation, RowExclusiveLock);
...}
```