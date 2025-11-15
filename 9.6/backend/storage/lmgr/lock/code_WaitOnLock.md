#1.WaitOnLock

```cpp

static void
WaitOnLock(LOCALLOCK *locallock, ResourceOwner owner)  //等待、以获取锁
{...
        if (ProcSleep(locallock, lockMethodTable) != STATUS_OK)
        //有死锁发生，调用DeadLockReport()详细汇报死锁信息
        {... }
...
}
```