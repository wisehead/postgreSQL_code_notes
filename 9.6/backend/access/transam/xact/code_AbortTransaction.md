#1.AbortTransaction

```cpp
static void
AbortTransaction(void)
{...
    /* Make sure we have a valid memory context and resource owner */
    AtAbort_Memory();           //切换事务所使用的内存上下文
    AtAbort_ResourceOwner();    //切换资源属主
    /* Release any LW locks we might be holding as quickly as possible.
     * (Regular locks, however, must be held till we finish aborting.)
     * Releasing LW locks is critical since we might try to grab them again while cleaning up!
     */
    LWLockReleaseAll();        //释放轻量锁，这是一种系统锁，非事务类型的锁，参见8.2.2节
    /* Clear wait information and command progress indicator */
    pgstat_report_wait_end();
    pgstat_progress_end_command();
    /* Clean up buffer I/O and buffer context locks, too */
    AbortBufferIO();
    UnlockBuffers();
    XLogResetInsertion();/* Reset WAL record construction state */
    /* Also clean up any open wait for lock, since the lock manager will choke
     * if we try to wait for another lock before doing this. */
    LockErrorCleanup();//锁的清理，如本事务所申请的锁正在等待其他事务释放，则需要把本事务
    所申请的锁从等待队列中删除
    /* If any timeout events are still active, make sure the timeout interrupt is scheduled.
     * This covers possible loss of a timeout interrupt due to longjmp'ing out of
the SIGINT handler (see notes in handle_sig_alarm).
     * We delay this till after LockErrorCleanup so that we don't uselessly
  reschedule lock or deadlock check timeouts. */
    reschedule_timeouts();  //清理超时机制所遗留的
...
    //set the current transaction state information appropriately during the abort processing
    s->state = TRANS_ABORT;  //设置事务的回滚状态，表明回滚操作已经生效
...
    //do abort processing, 如下一些工作类似于事务提交阶段所做的环节，只是每类操作侧重于“abort”
    AfterTriggerEndXact(false); /* 'false' means it's abort */
    AtAbort_Portals();
    AtEOXact_LargeObject(false);
    AtAbort_Notify();
    AtEOXact_RelationMap(false);
    AtAbort_Twophase();
    if (!is_parallel_worker)
        latestXid = RecordTransactionAbort(false); //注意回滚操作要调用
XactLogAbortRecord()记录到REDO日志中，然后调用TransactionIdAbortTree()
在clog中标识事务撤销
    else
    {...
        XLogSetAsyncXactLSN(XactLastRecEnd);   //并行方式的处理
    }
...
    if (TopTransactionResourceOwner != NULL)
    //如果是顶层事务，最重要的一个步骤，是释放锁的过程
    {...
        ResourceOwnerRelease(TopTransactionResourceOwner,
RESOURCE_RELEASE_BEFORE_LOCKS, false, true);
        AtEOXact_Buffers(false);   //清理缓存区
...
        ResourceOwnerRelease(TopTransactionResourceOwner, RESOURCE_RELEASE_LOCKS,
 false, true); //释放锁
        ResourceOwnerRelease(TopTransactionResourceOwner, RESOURCE_RELEASE_AFTER_
LOCKS, false, true);
...
        AtEOXact_GUC(false, 1);    //清理GUC参数
...
    }
...
}


```