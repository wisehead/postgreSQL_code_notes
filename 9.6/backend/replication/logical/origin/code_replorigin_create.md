#1.replorigin_create

```cpp
3.为系统表加锁
replorigin_create(char *roname)
{...
    rel = heap_open(ReplicationOriginRelationId, ExclusiveLock);
    // ReplicationOriginRelationId，pg_replication_origin系统表的标识
...}
```