#1.RecordTransactionCommit

```cpp
static TransactionId
RecordTransactionCommit(void)
{...
    if (!markXidCommitted) //事务ID不合法
    {...}
    else
    {...
        START_CRIT_SECTION();
        MyPgXact->delayChkpt = true;
        SetCurrentTransactionStopTimestamp();
        XactLogCommitRecord(xactStopTimestamp, nchildren, children, nrels, rels,
 nmsgs, invalMessages,  //记录提交信息到日志
                            RelcacheInitFileInval, forceSyncCommit,
 InvalidTransactionId /* plain commit */ );
...
        //记录提交时间点的信息到日志（调用XLogInsert(RM_COMMIT_TS_ID, COMMIT_TS_SETTS)
把时间点标志信息写入日志），并计载clog
        TransactionTreeSetCommitTsData(xid, nchildren, children, replorigin_
session_origin_timestamp, replorigin_session_origin, false);
    }
    if ((wrote_xlog && markXidCommitted && synchronous_commit > SYNCHRONOUS_
    COMMIT_OFF) || forceSyncCommit || nrels > 0)
    {
        XLogFlush(XactLastRecEnd);  //刷出WAL日志
        if (markXidCommitted)
            TransactionIdCommitTree(xid, nchildren, children);
            //更新clog，把事务完成的信息写入clog
    }
    else
    {...}   //异步提交
...
    /* Compute latestXid while we have the child XIDs handy */
    latestXid = TransactionIdLatest(xid, nchildren, children);
    //注意返回值latestXid在上层调用函数中的用法
...
    return latestXid;
}

```