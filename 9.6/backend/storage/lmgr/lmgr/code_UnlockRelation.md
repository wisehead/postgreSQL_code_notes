#1.UnlockRelation

```cpp

void
UnlockRelation(Relation relation, LOCKMODE lockmode)
{...
    SET_LOCKTAG_RELATION(tag,  //设置关系的标志
                  relation->rd_lockInfo.lockRelId.dbId, relation->rd_lockInfo.lockRelId.relId);
    LockRelease(&tag, lockmode, false); //然后通过“LockRelease”完成释放锁操作
}
```