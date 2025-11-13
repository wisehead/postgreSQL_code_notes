#1.LWLockAcquire

```cpp

LWLockAcquire(LWLock *lock, LWLockMode mode)
//acquire a lightweight lock(第一个参数lock) in the specified mode（第二个参数）
{...
    AssertArg(mode == LW_SHARED || mode == LW_EXCLUSIVE); //只能通过本函数申请两种加锁模式
...
    for (;;)  //如不能获得锁，则反复循序。只有第一次或第二次加锁成功才能退出循环
    {...
        mustwait = LWLockAttemptLock(lock, mode);
        //试图获取锁，如果不能获得则不等待。这个函数的实现调用了原子操作如pg_atomic_read_u32()
        if (!mustwait) //已经获得了锁
        {
            LOG_LWDEBUG("LWLockAcquire", lock, "immediately acquired lock");
            break;                /* got the lock */  //退出循环
        } //否则，往下继续执行，意味着没有获得锁
        LWLockQueueSelf(lock, mode);
        //没有获得锁，把申请锁的对象（锁的未来属主）加入锁等待队列的队尾
        mustwait = LWLockAttemptLock(lock, mode); //尝试第二次获取锁
        if (!mustwait) //第二次获取锁的尝试成功，即加锁成功
        {
            LOG_LWDEBUG("LWLockAcquire", lock, "acquired, undoing queue");
            LWLockDequeueSelf(lock);
            //第二次获取锁的尝试成功,需要把申请锁的对象（已经是锁的属主）从等待队列中去除
            break; //退出循环
        }
...
        LWLockReportWaitStart(lock);
        //第二次获取锁的尝试失败，则向当前进程汇报加锁失败的消息(给进程结构体上的
        wait_event_info赋值)
...
        for (;;)
        {
            PGSemaphoreLock(&proc->sem);
            //使用信号量阻塞本次加锁请求，直至被唤醒（可能其他属主释放了本锁）
            if (!proc->lwWaiting) //被唤醒
                break;
            extraWaits++;
        }
...
        /* Retrying, allow LWLockRelease to release waiters again. */
        pg_atomic_fetch_or_u32(&lock->state, LW_FLAG_RELEASE_OK);
        //第三次尝试，看看state上是否已经被释放锁
...
        LWLockReportWaitEnd(); //向当前进程汇报加锁操作不再需要等待
        //(给进程结构体上的wait_event_info赋值为“0”)
...
        /* Now loop back and try to acquire lock again. */
        result = false; //循环重新执行，重新尝试以上的过程
    }
...
    /* Add lock to list of locks held by this backend */
    held_lwlocks[num_held_lwlocks].lock = lock;
    //记录加到的锁的信息到当前进程的“锁列表”(held_lwlocks)里
    held_lwlocks[num_held_lwlocks++].mode = mode;
...
}
```