#1.HeapTupleSatisfiesMVCC

```cpp

bool  //元组的版本htup是否可以被参数snapshot所代表事务可见？ 返回TRUE表示版本可见
HeapTupleSatisfiesMVCC(HeapTuple htup, Snapshot snapshot, Buffer buffer)
//注意，元组htup是谁生成的需要与快照的关系不大，除非是本事务自己生成的元组，需要和快照结合判断
  本事务内对子事务的可见性
{...
    //第一种情况：元组已经生成，但是还没有提交。插入操作进行中。
    if (!HeapTupleHeaderXminCommitted(tuple)) //对于没有提交的元组，判断可见性。包括
    正在插入和正在更新的情况
    {
        if (HeapTupleHeaderXminInvalid(tuple)) //元组不合法，坏的元组
            return false;
        /* Used by pre-9.0 binary upgrades */
        if (tuple->t_infomask & HEAP_MOVED_OFF)
        //适用于把9.0版本之前的二进制数据目录升级到9.0版本及之后的版本的数据
        {...}
        /* Used by pre-9.0 binary upgrades */
        else if (tuple->t_infomask & HEAP_MOVED_IN)  //同上
        {...}
        else if (TransactionIdIsCurrentTransactionId(HeapTupleHeaderGetRawXmin(tuple)))
        //如果是本事务生成的元组
        {
            //本事务快照建立之后，本事务插入的数据，对于事务刚开始时即建立的快照而言，是不可见的
            if (HeapTupleHeaderGetCmin(tuple) >= snapshot->curcid)
            //但这种情况可能发生吗？本事务生成的数据对本事务可见，即读取本事务的数据是不需要快照
              的。以，这只可能发生在并发执行的多个子事务之间
                return false;    /* inserted after scan started */
            if (tuple->t_infomask & HEAP_XMAX_INVALID)  /* xid invalid */
            //元组被更新或删除，但执行更新或删除的事务被回滚，则元组对于本事务而言是可见的
                return true;
            if (HEAP_XMAX_IS_LOCKED_ONLY(tuple->t_infomask))  /* not deleter */
            //元组只是被锁住，但还没有发生删除、修改等操作，故可见
                return true;
            if (tuple->t_infomask & HEAP_XMAX_IS_MULTI)
            //版本是其他事务执行更新操作修改过的（删除的旧版本）
            {   //--元组是本事务生成的，但被其他事务更新。这可能发生吗？----
                  只可能发生在并发的子事务间
                TransactionId xmax;
                xmax = HeapTupleGetUpdateXid(tuple);
               //取到这个版本的xmax（因为版本是其他事务执行更新操作生成的）
...
                /* updating subtransaction must have aborted */
                if (!TransactionIdIsCurrentTransactionId(xmax))
                //不是本事务删除此元组，生产此版本的更新子事务被回滚，则对于本事务是可见的
                    return true;
                else if (HeapTupleHeaderGetCmax(tuple) >= snapshot->curcid)
                //更新是快照之后发生，则对于本事务是可见的
                    return true;    /* updated after scan started */
                else
                    return false;   /* updated before scan started */
                //更新是快照之前发生，则对于本事务是不可见的
            }
            if (!TransactionIdIsCurrentTransactionId(HeapTupleHeaderGetRawXmax(tuple)))
            //执行删除操作的子事务被回顾，删除无效
            {
                /* deleting subtransaction must have aborted */
                SetHintBits(tuple, buffer, HEAP_XMAX_INVALID, InvalidTransactionId);
                return true;  //执行删除操作的子事务被回顾，删除无效，故可见
            }
            if (HeapTupleHeaderGetCmax(tuple) >= snapshot->curcid)
                return true;    /* deleted after scan started */
               //快照之后发生删除的元组，对本事务可见
            else
                return false;   /* deleted before scan started */
                //快照之前发生删除的元组，对本事务不可见
        }
        else if (XidInMVCCSnapshot(HeapTupleHeaderGetRawXmin(tuple), snapshot))
                //快照范围之内发生的，不可见
            return false;
        else if (TransactionIdDidCommit(HeapTupleHeaderGetRawXmin(tuple)))
            SetHintBits(tuple, buffer, HEAP_XMIN_COMMITTED, HeapTupleHeaderGetRawXmin(tuple));
//即不是本事务生产的数据，也不在快照的范围内部，则只有两种情况，一是在快照的左边界之左面，属于可
见范围，二是在快照的右边界之右面，属于不可见范围。对于前者，此处不做可见性判断，留待如下借用“第
三种情况”的代码进行可见性判断。对于后者，对应的就是下面的else内容，一定不可见
        else   //已被回滚或发生过crashed的元组，对本事务不可见
        {
            /* it must have aborted or crashed */
            SetHintBits(tuple, buffer, HEAP_XMIN_INVALID, InvalidTransactionId);
            return false;   //二是在快照的右边界之右面，属于不可见范围，一定不可见
        }
    }
    else  //第二种情况：元组已经生成，已经提交
    {
        /* xmin is committed, but maybe not according to our snapshot */
          //对于已经提交的元组，判断可见性
        if (!HeapTupleHeaderXminFrozen(tuple) &&  //元组没有被冻结（已经提交的、不
        合法的才能被冻结），所以结合上面的else进入的条件，此处是在对没有冻结的元组进行判断
            XidInMVCCSnapshot(HeapTupleHeaderGetRawXmin(tuple), snapshot))
            //且元组快照范围之内发生的，不可见
            return false;        /* treat as still in progress */
    }
    //第三种情况：元组已经提交，几乎应该都可见，但是有例外情况
    /* by here, the inserting transaction has committed */
    //前面判断的是正在执行的，所以正在执行的情况已经判断完毕
    if (tuple->t_infomask & HEAP_XMAX_INVALID)    /* xid invalid or aborted */
    //已经提交且没有被删除，则可见
        return true;
    if (HEAP_XMAX_IS_LOCKED_ONLY(tuple->t_infomask))    //元组只是被锁住，则可见
        return true;
    //第四种情况：元组被别的事务更新或删除，但这样的事务没有完成
    if (tuple->t_infomask & HEAP_XMAX_IS_MULTI)
    //此版本上发生了并发的更新或删除操作
    {...
        xmax = HeapTupleGetUpdateXid(tuple);
...
        if (TransactionIdIsCurrentTransactionId(xmax))
        //更新或删除操作致使老版本置了xmaz值，判断是否是当前事务所为
        {
            if (HeapTupleHeaderGetCmax(tuple) >= snapshot->curcid)
            //本事务内部，后发生的删除操作对删除操作之前快照是可见的
                return true;    /* deleted after scan started */
            else
                return false;    /* deleted before scan started */
        }
        if (XidInMVCCSnapshot(xmax, snapshot)) //更新操作是当前事务所为，且在快照范围内，可见
            return true;
        if (TransactionIdDidCommit(xmax))
           //更新操作是当前事务所为，但已经提交，则不可见（更新操作是个子事务）
            return false;        /* updating transaction committed */
        /* it must have aborted or crashed */
        return true;
    }
    if (!(tuple->t_infomask & HEAP_XMAX_COMMITTED))  //此版本是最新版本，没有被更新或删除
    {
        if (TransactionIdIsCurrentTransactionId(HeapTupleHeaderGetRawXmax(tuple)))
         //当前事务所为
        {
            if (HeapTupleHeaderGetCmax(tuple) >= snapshot->curcid)
             //本事务内部，后发生的删除操作对删除操作之前快照是可见的
                return true;    /* deleted after scan started */
            else
                return false;    /* deleted before scan started */
        }
        if (XidInMVCCSnapshot(HeapTupleHeaderGetRawXmax(tuple), snapshot))
        //更新操作是当前事务所为，且在快照范围内，可见
            return true;
        if (!TransactionIdDidCommit(HeapTupleHeaderGetRawXmax(tuple)))
           //操作已经回滚，则不可见（更新操作是个子事务）
        {
            /* it must have aborted or crashed */
            SetHintBits(tuple, buffer, HEAP_XMAX_INVALID, InvalidTransactionId);
            return true;
        }
        /* xmax transaction committed */
        SetHintBits(tuple, buffer, HEAP_XMAX_COMMITTED, HeapTupleHeaderGetRawXmax(tuple));
    }
    else  //第五种情况：元组被别的事务更新或删除，这样的事务已经完成。
    {
        /* xmax is committed, but maybe not according to our snapshot */
        if (XidInMVCCSnapshot(HeapTupleHeaderGetRawXmax(tuple), snapshot))
            return true;        /* treat as still in progress */
    }
    /* xmax transaction committed */
    return false;
}

```