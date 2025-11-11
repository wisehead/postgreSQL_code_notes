#1.CleanupTransaction

```cpp


static void
CleanupTransaction(void)
{...
    AtCleanup_Portals();             /* now safe to release portal memory */
    AtEOXact_Snapshot(false);        /* and release the transaction's snapshots */
    CurrentResourceOwner = NULL;     /* and resource owner */
    if (TopTransactionResourceOwner)
        ResourceOwnerDelete(TopTransactionResourceOwner);
    s->curTransactionOwner = NULL;
    CurTransactionResourceOwner = NULL;
    TopTransactionResourceOwner = NULL;
    AtCleanup_Memory();              //清理事务过程中所使用的内存，如把分配过的内存回收
    s->transactionId = InvalidTransactionId;
    s->subTransactionId = InvalidSubTransactionId;
    s->nestingLevel = 0;             //重置全局的一些变量
    s->gucNestLevel = 0;
    s->childXids = NULL;
    s->nChildXids = 0;
    s->maxChildXids = 0;
    s->parallelModeLevel = 0;
...
    s->state = TRANS_DEFAULT;        //标识回滚操作完成
}


```