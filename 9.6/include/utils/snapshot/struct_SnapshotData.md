#1.struct SnapshotData

```cpp
typedef struct SnapshotData  //Struct representing all kind of possible snapshots
{
    //“satisfies”不同的快照有不同的类型，可能的值为：HeapTupleSatisfiesMVCC /
    HeapTupleSatisfiesHistoricMVCC
    SnapshotSatisfiesFunc satisfies;    /* tuple test function */
    //在事务发生这根线上（时间轴），不断有事务产生、提交或回滚，这表明每一个事务在事务发生这根
线上都是一段，这样的段有多个，多个间有重叠有分离，但是，从一个时间段上看，可能看到零个事务、一个
事务或多个事务，这样的时间段就是一个快照。所以说，快照是事务发生线条上的一个线段，所以有比这个线
段起始点早发生的（xmin之前即小于xmin），也有比这个线段结束点之后发生的（xman之后即大于xmax）
    TransactionId xmin; /* all XID < xmin are visible to me */
    //事务的ID小于快照的左边界xmin，则其对应的数据被快照持有者的事务可见
    TransactionId xmax; /* all XID >= xmax are invisible to me */
    //事务的ID大于快照的右边界xmax，则其对应的数据不被快照持有者的事务可见
    TransactionId *xip;   //对于“normal MVCC”类型的快照，表示处于活动状态的事务的指针
    数组；对于“historic MVCC”，表示已经提交的事务的指针数组
    uint32        xcnt;  /* # of xact ids in xip[] */ //xip的计数器
    /*
     * For non-historic MVCC snapshots, this contains subxact IDs that are in
     * progress (and other transactions that are in progress if taken during
     * recovery). For historic snapshot it contains *all* xids assigned to the
     * replayed transaction, including the toplevel xid.
     *
     * note: all ids in subxip[] are >= xmin, but we don't bother filtering out
     any that are >= xmax
     */
    TransactionId *subxip; //子事务相关的事务ID信息
    int32         subxcnt;        /* # of xact ids in subxip[] */  //子事务相关的事务ID信息的计数器
    bool          suboverflowed;  /* has the subxip array overflowed? */
    bool          takenDuringRecovery;  /* recovery-shaped snapshot? */
    bool          copied;               /* false if it's a static snapshot */
...
    /*
     * Book-keeping information, used by the snapshot manager
     */
    uint32        active_count;    /* refcount on ActiveSnapshot stack */
    uint32        regd_count;      /* refcount on RegisteredSnapshots */
    pairingheap_node ph_node;      /* link in the RegisteredSnapshots heap */
    int64         whenTaken;       /* timestamp when snapshot was taken */
      //生成快照时的时间戳值
    XLogRecPtr    lsn;             /* position in the WAL stream when taken */
    //生成快照时的LSN
} SnapshotData;

```