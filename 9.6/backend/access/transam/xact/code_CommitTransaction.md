#1.CommitTransaction

```cpp
CommitTransaction(void)  //执行事务提交时，事务收尾工作
{...
    //执行被延迟的触发器
...
    if (IsInParallelMode())
        AtEOXact_Parallel(true);  //清理并行相关内容
    AfterTriggerEndXact(true);  /* Shut down the deferred-trigger manager */
    PreCommit_on_commit_actions(); //为临时表处理“ON COMMIT”操作
    /* close large objects before lower-level cleanup */
    AtEOXact_LargeObject(true);  //关闭大对象，大对象在PG中被拆分为多个小的元组存储
   PreCommit_CheckForSerializationFailure(); //事务提交前，对可串行化事务进行合法性检
   查，详情参见9.4.5节
    PreCommit_Notify(); // Insert notifications sent by NOTIFY commands into the queue.
    HOLD_INTERRUPTS();  /* Prevent cancel/die interrupt while cleaning up */
    //执行到此处，不能被撤销，只能继续向下执行
    /* Commit updates to the relation map --- do this as late as possible */
   AtEOXact_RelationMap(true); //调用perform_relmap_update()->write_relmap_file()
   更新系统关系表，更新前，需要把恢复信息写入日志以备不测
    s->state = TRANS_COMMIT;    //设置为提交状态，这是事务提交的标志。之后，才能释放锁等
    s->parallelModeLevel = 0;
    if (!is_parallel_worker)
    {
        //We need to mark our XIDs as committed in pg_clog.  This is where we durably commit.
        latestXid = RecordTransactionCommit();  //在clog中记载事务提交这个事实
    }
    else
    {...}  //处理并行的情况
    TRACE_POSTGRESQL_TRANSACTION_COMMIT(MyProc->lxid);
    ProcArrayEndTransaction(MyProc, latestXid); //通知其他进程，本进程没有事务在执行了
    /* The ordering of operations is not entirely random.  The idea is:
    //注意清理的顺序如下
     * release resources visible to other backends (eg, files, buffer pins);
then release locks; then release backend-local resources.
     * We want to release locks at the point where any backend waiting for us
     will see
     * our transaction as being fully cleaned up. */
    CallXactCallbacks(is_parallel_worker ? XACT_EVENT_PARALLEL_COMMIT :
XACT_EVENT_COMMIT); //提交操作之后，通过钩子函数，执行的清理工作。目前主要用于外部表和标记
安全相关的操作。如果不涉及外部表和标记安全这两个特性，本地事务则不需要执行此回调函数规定的清理
顺序，所以特别注意不要和本地事务的清理工作相混淆（一个重要的混淆点，在于锁的释放是在事务标识
为提交之后才进行，则误以为PostgreSQL使用了两阶段封锁协议，此处实则是对于外部表和标记安全相关
的操作而言的。而PostgreSQL自身的并发管理是使用的SS2PL和MVCC，记录锁的施加是在用后即释放的，
如在heap_update()函数中调用heap_acquire_tuplock()->LockTuple()加锁，然后在这个函数内
调用UnlockTuple()释放掉锁；而DDL类的锁确实要被释放掉，只是稍候释放）
    ResourceOwnerRelease(TopTransactionResourceOwner, RESOURCE_RELEASE_
    BEFORE_LOCKS, true, true); //资源的释放
    AtEOXact_Buffers(true); /* Check we've released all buffer pins */
    AtEOXact_RelationCache(true); /* Clean up the relation cache */
    AtEOXact_Inval(true); // Make catalog changes visible to all backends.
    AtEOXact_MultiXact();
    //调用ProcReleaseLocks()->LockReleaseAll()释放锁，调用ReleasePredicateLocks()
释放谓词锁（SSI技术实现序列化，参见第9章）。从此处看，PostgreSQL使用了SS2PL技术，在提交点
之后才释放锁。但是，需要注意的是，这里释放的都是DDL类型的事务锁，而不包括DML类型的事务锁。所
以PostgreSQL使用的SS2PL技术只是针对DDL类操作，而第9章讲述的MVCC则是DML操作并发控制技术。例
如，更新操作的元组锁的释放是在update操作执行函数heap_update()结束前就释放的，参见8.3.3节
“3.行级锁的加锁和释放”。
    ResourceOwnerRelease(TopTransactionResourceOwner,
    RESOURCE_RELEASE_LOCKS, true, true);
    ResourceOwnerRelease(TopTransactionResourceOwner, RESOURCE_RELEASE_AFTER_
LOCKS, true, true); //依序释放资源如catcache、plancache、snapshot、临时文件、索引扫描
清理等
    smgrDoPendingDeletes(true);                //事务提交，删除relation、清理物理存储层
    AtEOXact_CatCache(true);      /* Check we've released all catcache entries */
    AtCommit_Notify();                          //事务提交，清理内存
    AtEOXact_GUC(true, 1);                      //事务提交，清理GUC变量等
...
    AtEOXact_Files();                           //事务提交，清理文件
...
    AtEOXact_PgStat(true);                      //事务提交，清理统计
    AtEOXact_Snapshot(true);                    //事务提交，清理快照
    pgstat_report_xact_timestamp(0);
...
    AtCommit_Memory();                         //事务提交，清理内存
    s->transactionId = InvalidTransactionId;   //事务的属性回归初始值
...
    s->state = TRANS_DEFAULT;                  //事务提交完成
    RESUME_INTERRUPTS();
}

```