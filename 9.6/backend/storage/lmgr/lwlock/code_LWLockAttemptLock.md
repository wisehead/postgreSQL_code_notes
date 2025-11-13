#1.LWLockAttemptLock

```cpp

LWLockAttemptLock(LWLock *lock, LWLockMode mode)
{...
    old_state = pg_atomic_read_u32(&lock->state);  //读出锁的原始值
    while (true)
    {...
        desired_state = old_state; //desired_state上先保持旧值
        if (mode == LW_EXCLUSIVE)
        {
            lock_free = (old_state & LW_LOCK_MASK) == 0;
            if (lock_free)
                desired_state += LW_VAL_EXCLUSIVE;
                //desired_state上先保持旧值后，根据锁的模式加排它锁的标识
        }
        else
        {
            lock_free = (old_state & LW_VAL_EXCLUSIVE) == 0;
            if (lock_free)
                desired_state += LW_VAL_SHARED;
                //desired_state上先保持旧值后，根据锁的模式加共享锁的标识
        }
        //desired_state成为在锁标志位上将被设置的值。pg_atomic_compare_exchange_u32()
          完成设置标志位（不同硬件平台，设置方式不同）
        if (pg_atomic_compare_exchange_u32(&lock->state, &old_state, desired_state))
        {...}
    }
...
}
```