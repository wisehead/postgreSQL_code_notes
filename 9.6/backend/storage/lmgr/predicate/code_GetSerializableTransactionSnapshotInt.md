#1.GetSerializableTransactionSnapshotInt

```cpp
static Snapshot
GetSerializableTransactionSnapshotInt(Snapshot snapshot, TransactionId sourcexid)
{...
    LWLockAcquire(SerializableXactHashLock, LW_EXCLUSIVE);
    do
    {
        sxact = CreatePredXact();
        //从定长的一个列表中（PredXact->availableList）取出一个可以使用的可串行化事务
        /* If null, push out committed sxact to SLRU summary & retry. */
        if (!sxact) //如果定量的事务配额用光，则汇总一部分已经提交了的事务为一个事务
        {
            LWLockRelease(SerializableXactHashLock);
            SummarizeOldestCommittedSxact(); //汇总一部分已经提交了的事务为一个事务
            LWLockAcquire(SerializableXactHashLock, LW_EXCLUSIVE);
        }
    } while (!sxact);
    /* Get the snapshot, or check that it's safe to use */
    if (!TransactionIdIsValid(sourcexid))
    //对于可串行化快照的获取，本函数的入口参数传入的是“InvalidTransactionId”，
        snapshot = GetSnapshotData(snapshot); //意味着一定会调用GetSnapshotData()来
获得一个物理快照，而此函数同样在可重复读隔离级别下被调用，表明可串行化隔离级别是比可可重复读多的
是SSI技术下的读写冲突检测而非快照的获取方式，由此体会这两个隔离级别实现的差异----即本段代码之前
和之后所做的多种判断工作，正是可串行化隔离级别需要的必要的工作
    else if (!ProcArrayInstallImportedXmin(snapshot->xmin, sourcexid))
    {...}
    if (XactReadOnly && PredXact->WritableSxactCount == 0) //如果本事务是只读的，且
    当前不存在写事务，则不可能构成读写重复，因此可不在可串行化隔离级别下运行，直接返回快照
    {
        ReleasePredXact(sxact);
        LWLockRelease(SerializableXactHashLock);
        return snapshot;
    }
...
    /* Initialize the structure. */
    sxact->vxid = vxid;
    sxact->SeqNo.lastCommitBeforeSnapshot = PredXact->LastSxactCommitSeqNo;
...
    if (XactReadOnly)  //如果本事务是只读的，则判断是否可能存在读写冲突，判断条件如下
    {
        sxact->flags |= SXACT_FLAG_READ_ONLY;
        /* Register all concurrent r/w transactions as possible conflicts;
         * if all of them commit without any outgoing conflicts to earlier transactions
         * then this snapshot can be deemed safe (and we can run without tracking
           predicate locks). */
        for (othersxact = FirstPredXact(); othersxact != NULL; othersxact =
            NextPredXact(othersxact)) //遍历所有的活动事务
        {
            if (!SxactIsCommitted(othersxact)     //活动的事务：即不是已经提交的
                && !SxactIsDoomed(othersxact)     //活动的事务：也不是已经注定要回滚的
                && !SxactIsReadOnly(othersxact))  //活动的事务：还不是只读的，那么这样
                                                    的事务就可能与本事务存在读写冲突
            {
                SetPossibleUnsafeConflict(sxact, othersxact); //记载可能的冲突关系，
                                                                将来进一步由
            }
        }
    }
    else   //本事务不是只读的
    {
        ++(PredXact->WritableSxactCount);  //对存在的写事务计数
        Assert(PredXact->WritableSxactCount <= (MaxBackends + max_prepared_xacts));
    }
...
    LocalPredicateLockHash = hash_create("Local predicate lock",
    //创建本会话/进程独享的本地谓词锁的Hash表，便于将来快速搜索
                max_predicate_locks_per_xact, &hash_ctl, HASH_ELEM | HASH_BLOBS);
    return snapshot;
}
```