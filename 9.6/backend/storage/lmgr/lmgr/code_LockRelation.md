#1.LockRelation

```cpp
void
LockRelation(Relation relation, LOCKMODE lockmode)
{...
    SET_LOCKTAG_RELATION(tag,  //设置关系的标志
            relation->rd_lockInfo.lockRelId.dbId, relation->rd_lockInfo.lockRelId.relId);
    res = LockAcquire(&tag, lockmode, false, false); //然后通过“LockAcquire”完成加锁操作
...
}

```