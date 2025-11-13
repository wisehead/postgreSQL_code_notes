#1.LWLockRelease

```cpp
void
LWLockRelease(LWLock *lock) //释放锁并唤醒等待者
{...
    for (i = num_held_lwlocks; --i >= 0;)
//准备从held_lwlocks中释放锁，为此锁找到当初的加锁模式
    {
        if (lock == held_lwlocks[i].lock)
        {
            mode = held_lwlocks[i].mode;
            break;
        }
    }
...
    if (mode == LW_EXCLUSIVE) //释放锁，即给锁标志位置位（还原为初始值）
        oldstate = pg_atomic_sub_fetch_u32(&lock->state, LW_VAL_EXCLUSIVE);
    else
        oldstate = pg_atomic_sub_fetch_u32 (&lock->state, LW_VAL_SHARED);
...
    if (check_waiters)
    {
        /* XXX: remove before commit? */
        LOG_LWDEBUG("LWLockRelease", lock, "releasing waiters");
        LWLockWakeup(lock); //如果此锁上有等待这，唤醒等待者（告诉操作系统重新调度进程）
    }
...
}
```