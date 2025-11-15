#1.AcquireDeletionLock

```cpp
AcquireDeletionLock(const ObjectAddress *object, int flags) //在删除的对象上加排它类的锁
{
    if (object->classId == RelationRelationId) //如果是pg_class系统表
    {
        /*
         * In DROP INDEX CONCURRENTLY, take only ShareUpdateExclusiveLock on
         * the index for the moment.  index_drop() will promote the lock once
         * it's safe to do so.  In all other cases we need full exclusive
         * lock.
         */
        if (flags & PERFORM_DELETION_CONCURRENTLY)
        //如果是DROP INDEX CONCURRENTLY操作，在索引上加可并发访问的排它锁
            LockRelationOid(object->objectId, ShareUpdateExclusiveLock);
            //有部分新请求的锁可以在此锁基础上升级
        else
            LockRelationOid(object->objectId, AccessExclusiveLock);
            //最强级别的锁，没有新的锁可以再升级（即任何锁都被排斥）
    }
    else
    {
        /* assume we should lock the whole object not a sub-object */
        LockDatabaseObject(object->classId, object->objectId, 0, AccessExclusiveLock);
    }
}

```