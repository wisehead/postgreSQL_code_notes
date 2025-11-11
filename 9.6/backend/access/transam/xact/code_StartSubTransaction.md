#1.StartSubTransaction

```cpp
static void
StartSubTransaction(void)
{...
    s->state = TRANS_START;     //子事务同样以TRANS_START为子事务开始的状态标识
    AtSubStart_Memory();        //子事务的内存空间初始化
    AtSubStart_ResourceOwner(); //子事务的资源初始化
    AtSubStart_Notify();
    AfterTriggerBeginSubXact();
    s->state = TRANS_INPROGRESS;
...
}

```