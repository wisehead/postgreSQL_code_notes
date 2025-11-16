#1.GetSerializableTransactionSnapshotInt

```cpp
Snapshot
GetSerializableTransactionSnapshot(Snapshot snapshot)
{...
    /*
     * A special optimization is available for SERIALIZABLE READ ONLY DEFERRABLE transactions
     * -- we can wait for a suitable snapshot and thereby avoid all SSI overhead
      once it's running. */
    if (XactReadOnly && XactDeferrable)
    //如果事务是只读且是可延迟的，则一定不必在可串行化隔离级别下运行
        return GetSafeSnapshot(snapshot);
        //即使设置的隔离级别是可串行化隔离级别。即意味着只读可延迟事务一定不会发生读写冲突
    return GetSerializableTransactionSnapshotInt(snapshot,InvalidTransactionId);
     //否则，才获得一个可串行化事务快照
}
```