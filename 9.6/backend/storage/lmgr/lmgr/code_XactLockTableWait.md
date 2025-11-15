#1.XactLockTableWait
“ShareLock”锁主要在创建索引时使用，但也有特殊用法，如：

```cpp
XactLockTableWait(TransactionId xid, Relation rel, ItemPointer ctid,
//等待指定的事务提交/回滚操作完成
                   XLTW_Oper oper)
{...
    for (;;)
    {...
        SET_LOCKTAG_TRANSACTION(tag, xid); //xid，指定的事务
        (void) LockAcquire(&tag, ShareLock, false, false);
         //为了等待指定的事务提交/回滚操作完成，不断加锁（下行解锁）空转
        LockRelease(&tag, ShareLock, false);
...
    }
...
}

```