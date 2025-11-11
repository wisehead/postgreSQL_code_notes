#1.TransactionIdDidCommit

```cpp
bool   /* true if given transaction committed */    //用于判断事务是否已经提交
TransactionIdDidCommit(TransactionId transactionId)
{...
    xidstatus = TransactionLogFetch(transactionId);  //根据事务ID获取本事务的状态
    if (xidstatus == TRANSACTION_STATUS_COMMITTED)
//如果在clog里面记录了事务ID的状态为提交，则表示事务已经提交
        return true;
...
    return false;  // It's not committed.
}

```