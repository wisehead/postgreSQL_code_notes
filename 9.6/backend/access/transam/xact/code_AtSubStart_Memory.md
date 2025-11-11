#1.AtSubStart_Memory

```cpp
static void
AtSubStart_Memory(void)  //子事务的内存空间初始化，是以当前事务的内存空间为根，
然后在当前事务的内存空间之下分配各自的空间
{...
    CurTransactionContext = AllocSetContextCreate(CurTransactionContext,
    //以当前事务的内存空间为根
        "CurTransactionContext", ALLOCSET_DEFAULT_MINSIZE, ALLOCSET_DEFAULT_
        INITSIZE, ALLOCSET_DEFAULT_MAXSIZE);
...
}
static void
AtStart_Memory(void) //父事务的内存空间初始化，是以顶层的内存空间为根，然后在顶层的内存
空间之下分配各自的空间
{...
    if (TransactionAbortContext == NULL)
     //以顶层的内存空间为根，分配所有子事务都需要的事务回滚内存上下文
        TransactionAbortContext = AllocSetContextCreate(TopMemoryContext,
         "TransactionAbortContext", 32 * 1024, 32 * 1024, 32 * 1024);
    TopTransactionContext =
        AllocSetContextCreate(TopMemoryContext,
        //以顶层的内存空间为根，分配所有父事务的内存上下文
                           "TopTransactionContext", ALLOCSET_DEFAULT_MINSIZE,
                           ALLOCSET_DEFAULT_INITSIZE, ALLOCSET_DEFAULT_MAXSIZE);
...
}
```