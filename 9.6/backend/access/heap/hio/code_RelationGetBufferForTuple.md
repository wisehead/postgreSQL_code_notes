#1.RelationGetBufferForTuple

```cpp
为指定的Relation加锁（物理页面在缓存区的页）
RelationGetBufferForTuple(Relation relation, ...) //返回被pin住的且被排它锁锁住的buf页
{...
    if (needLock)
    {
        if (!use_fsm)
            LockRelationForExtension(relation, ExclusiveLock);
        else if (!ConditionalLockRelationForExtension(relation, ExclusiveLock))
        {
            /* Couldn't get the lock immediately; wait for it. */
            LockRelationForExtension(relation, ExclusiveLock); //在relation上加排它锁
...
            //If some other waiter has already extended the relation, we don't
              need to do so; just use the existing freespace.
            if (targetBlock != InvalidBlockNumber)
            {
                UnlockRelationForExtension(relation, ExclusiveLock);
                goto loop;
            }
...
        }
    }
...
}
```