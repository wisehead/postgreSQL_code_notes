#1.GetTransactionSnapshot

```cpp
Snapshot
GetTransactionSnapshot(void)  //根据隔离级别获取事务快照
{...
    if (HistoricSnapshotActive())  //有历史快照，则使用历史上定制的快照，因为逻辑复制
    技术的主备机制不支持可串行化隔离级别
    {
        Assert(!FirstSnapshotSet);
        return HistoricSnapshot;
    }
    if (!FirstSnapshotSet)//是否使用同一个快照？如果有FirstXactSnapshot，则使用同一个不
    再创建，以保证整个事务块内看到的都是一致的数据
    {...
        //注意下面对于不同隔离级别的快照的获取，都将调用GetSnapshotData()获取快照，只是不同
          隔离级别需要做不同的检查，或者
        if (IsolationUsesXactSnapshot())
        //隔离级别需要大于等于可重复读“REPEATABLE READ”，即至少是可重复读或可串行化
        {
            /* First, create the snapshot in CurrentSnapshotData */
            if (IsolationIsSerializable())  //隔离级别是可串行化
                CurrentSnapshot = GetSerializableTransactionSnapshot(&Current
                SnapshotData);//间接调用GetSnapshotData()获取快照
            else
                CurrentSnapshot = GetSnapshotData(&CurrentSnapshotData);
               //隔离级别是可重复读。直接调用GetSnapshotData()获取快照
            /* Make a saved copy */
            CurrentSnapshot = CopySnapshot(CurrentSnapshot);
            FirstXactSnapshot = CurrentSnapshot;
...
        }
        else
            CurrentSnapshot = GetSnapshotData(&CurrentSnapshotData);
            //隔离级别是已提交读“READ COMMITTED”
        /* Don't allow catalog snapshot to be older than xact snapshot. */
        CatalogSnapshotStale = true;
        FirstSnapshotSet = true;  //完成一个事务内第一次获取快照的任务
        return CurrentSnapshot;
    }
    if (IsolationUsesXactSnapshot()) //第一次之后再次获取快照的可重复读和可串行化隔离级
别，则不用再生成新的快照，使用老的快照即可。此处是区别于已提交读隔离级别的重要点之一，在一个事务
块内的多个SQL语句是否使用“同一个”快照（FirstXactSnapshot），是可重复读（包括可串行化）和
已提交读之间的最大差别
        return CurrentSnapshot;
    /* Don't allow catalog snapshot to be older than xact snapshot. */
    CatalogSnapshotStale = true;
    CurrentSnapshot = GetSnapshotData(&CurrentSnapshotData);
    //第一次之后再次获取快照的已提交读隔离级别，则生成新的快照。每次都生成新快照
    return CurrentSnapshot;
}

```