#1.LWLockConditionalAcquire

```cpp
LWLockConditionalAcquire(LWLock *lock, LWLockMode mode)
{...
    AssertArg(mode == LW_SHARED || mode == LW_EXCLUSIVE);
//只能通过本函数申请两种加锁模式
...
    //相比LWLockAcquire()函数，此处缺少循环，意味着加锁不成功时不会反复尝试
    mustwait = LWLockAttemptLock(lock, mode);
//试图获取锁，如果不能获得则不等待。这个函数的实现调用了原子操作如pg_atomic_read_u32()
...
    return !mustwait; //不管加锁成功还是失败，都不再尝试
}
```