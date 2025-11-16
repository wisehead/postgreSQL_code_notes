#1.heap_get_latest_tid

```cpp
void  //在指定的表（relation）中，根据快照查找元组，如果存在多个版本，则根据每个版本的
        t_ctid找下一条版本
heap_get_latest_tid(Relation relation, Snapshot snapshot, ItemPointer tid)
{...
     ctid = *tid;
    priorXmax = InvalidTransactionId;    /* cannot check first XMIN */
    for (;;)  //无限循环
    {...
        buffer = ReadBuffer(relation, ItemPointerGetBlockNumber(&ctid));
        //获取页面的物理地址、元组的偏移地址等
        LockBuffer(buffer, BUFFER_LOCK_SHARE);
        page = BufferGetPage(buffer);
...
        offnum = ItemPointerGetOffsetNumber(&ctid);
...
        /* OK to access the tuple */
        tp.t_self = ctid;  //构造元组的信息，准备获取这样的元组的其他信息、如t_xmin、t_
        xmax等以判断元组的可见性等
        tp.t_data = (HeapTupleHeader) PageGetItem(page, lp);
        tp.t_len = ItemIdGetLength(lp);
        tp.t_tableOid = RelationGetRelid(relation);
...
        valid = HeapTupleSatisfiesVisibility(&tp, snapshot, buffer);
        //根据快照判断元组的可见性
        CheckForSerializableConflictOut(valid, relation, &tp, buffer, snapshot);
        //根据SSI技术做读写冲突检查
        if (valid)  //如果合法
            *tid = ctid;
        if ((tp.t_data->t_infomask & HEAP_XMAX_INVALID) ||
        //元组不合法，要么是被回滚，要么是系统Crashed，不应该再找下去
            HeapTupleHeaderIsOnlyLocked(tp.t_data) ||
            //对元组所处的状态进行判断，如是否提交是否加锁等
            ItemPointerEquals(&tp.t_self, &tp.t_data->t_ctid))
            //如果t_ctid指向本元组自己，则找到，可以不再寻找
        {
            UnlockReleaseBuffer(buffer);
            break;  //退出循环
        }
        ctid = tp.t_data->t_ctid;  //上面条件如果不满足，则找版本链条中的下一条元组，
        而下一条元组是通过本元组的t_ctid指向的
        priorXmax = HeapTupleHeaderGetUpdateXid(tp.t_data);
        UnlockReleaseBuffer(buffer);
    } /* end of loop */
}


```