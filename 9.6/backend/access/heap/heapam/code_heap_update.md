#1.heap_update

```cpp
HTSU_Result
heap_update(Relation relation, ItemPointer otid, HeapTuple newtup,   //更新操作
            CommandId cid, Snapshot crosscheck, bool wait, HeapUpdateFailureData
            *hufd, LockTupleMode *lockmode)
{...
    HeapSatisfiesHOTandKeyUpdate(relation, hot_attrs, key_attrs, id_attrs,
    //根据是否存在对索引列的更新操作确定在元组上的加锁模式
                                 &satisfies_hot, &satisfies_key, //返回值
                                 &satisfies_id, &oldtup, newtup);
    if (satisfies_key)
    {
        *lockmode = LockTupleNoKeyExclusive; //不更新索引列
...
    }
    else
    {
        *lockmode = LockTupleExclusive; //更新索引列
...
    }
...
    if (result == HeapTupleInvisible) //被更新对象不可见，报错，事务回滚
    {...}
    else if (result == HeapTupleBeingUpdated && wait)
    //元组正被并发的其他事务更新，且参数wait指定需要等待
    {...
        if (infomask & HEAP_XMAX_IS_MULTI) //有并发事务在操作同一个元组
        {...
            if (DoesMultiXactIdConflict((MultiXactId) xwait, infomask,
            //是否存在并发的事务已经施加了同样的锁
                                        *lockmode))
            {...
                /* acquire tuple lock, if necessary */
                heap_acquire_tuplock(relation, &(oldtup.t_self), *lockmode,
                //锁兼容则可以直接加锁，注意施加的是元组锁
                                     LockWaitBlock, &have_tuple_lock);
                                     //LockWaitBlock表明发出加锁请求，等待锁被释放
...
            }
...
        }
        else if (TransactionIdIsCurrentTransactionId(xwait))
        //请求加锁者就是自己，则不必再加锁
        {...}
        else if (HEAP_XMAX_IS_KEYSHR_LOCKED(infomask) && key_intact)
        //请求加锁者只申请“key-share”而UPDATE不改变索引列，则不必再加锁
        {...}
        else //当以上都不是的时候，正是可以为元组加锁的时机
        {...
            heap_acquire_tuplock(relation, &(oldtup.t_self), *lockmode,
            //直接加锁，注意施加的是元组锁
                                 LockWaitBlock, &have_tuple_lock);
            XactLockTableWait(xwait, relation, &oldtup.t_self, XLTW_Update);
            //等待阻碍本事务的其他事务提交或回滚
...
        }
...
    }
...
    CheckForSerializableConflictIn(relation, &oldtup, buffer);
    //SSI机制，检查是否存在一个“rw-conflict”
...
    if (have_tuple_lock)
        UnlockTupleTuplock(relation, &(oldtup.t_self), *lockmode);
        //更新完毕，解锁（和SS2PL机制不同之处）。注意这里是DML类操作
...
    return HeapTupleMayBeUpdated;
}
```