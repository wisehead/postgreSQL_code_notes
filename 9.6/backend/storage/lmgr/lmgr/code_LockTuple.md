#1. LockTuple

```cpp
void
LockTuple(Relation relation, ItemPointer tid, LOCKMODE lockmode)
{...
    SET_LOCKTAG_TUPLE(tag,  //设置行锁标志
             relation->rd_lockInfo.lockRelId.dbId, relation->rd_lockInfo.lockRelId.relId,
             ItemPointerGetBlockNumber(tid), ItemPointerGetOffsetNumber(tid));
    (void) LockAcquire(&tag, lockmode, false, false);  //申请行锁
}
```