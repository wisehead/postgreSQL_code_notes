#1.LWLockAcquireOrWait

```cpp
/* LWLockAcquireOrWait - Acquire lock, or wait until it's free
 *
 * The semantics of this function are a bit funky.  If the lock is currently
 * free, it is acquired in the given mode, and the function returns true.  If
 * the lock isn't immediately free, the function waits until it is released
 * and returns false, but does not acquire the lock.
 *
 * This is currently used for WALWriteLock: when a backend flushes the WAL,
 * holding WALWriteLock, it can flush the commit records of many other
 * backends as a side-effect.  Those other backends need to wait until the
 * flush finishes, but don't need to acquire the lock anymore.  They can just
 * wake up, observe that their records have already been flushed, and return.
 */
bool
LWLockAcquireOrWait(LWLock *lock, LWLockMode mode)
//加锁过程与LWLockAcquire()相似，但语义不同
{...
    //NB: We're using nearly the same twice-in-a-row lock acquisition protocol as
 LWLockAcquire(). Check its comments for details.
    mustwait = LWLockAttemptLock(lock, mode); //试图获取锁，如果不能获得则不等待。这个
    函数的实现调用了原子操作如pg_atomic_read_u32()
    if (mustwait) //加锁不成功，把申请加锁的这个进程加入等待队列
    {
        LWLockQueueSelf(lock, LW_WAIT_UNTIL_FREE); //把申请加锁的这个进程加入等待队列
        mustwait = LWLockAttemptLock(lock, mode); //再次试图获取锁
        if (mustwait)
        {...
            LWLockReportWaitStart(lock);
           //向当前进程汇报加锁失败的消息(给进程结构体上的wait_event_info赋值)，表示开始等待
...
            for (;;)
            {
                PGSemaphoreLock(&proc->sem);
                //使用信号量阻塞本次加锁请求，直至被唤醒（可能其他属主释放了本锁
                if (!proc->lwWaiting)
                    break;  //被唤醒，则退出循环
                extraWaits++;
            }
...
            LWLockReportWaitEnd();  //结束等待
...
        }
        else
        {...
            LWLockDequeueSelf(lock); //第二次加锁成功，则把自己从等待队列中摘除
        }
    }
...
    return !mustwait;
}

```