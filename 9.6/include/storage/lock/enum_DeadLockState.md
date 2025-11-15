#1.enum_DeadLockState

```cpp
/* Deadlock states identified by DeadLockCheck() */
//死锁检测过程中，锁处于的可能的状态如下
typedef enum
{
    DS_NOT_YET_CHECKED,  /* no deadlock check has run yet */
                        //还没有进行死锁检测，检测前的一个临时状态
    DS_NO_DEADLOCK,      /* no deadlock detected */
    //不存在死锁
    DS_SOFT_DEADLOCK,    /* deadlock avoided by queue rearrangement */
              //通过等待队列的重新排队，死锁被避免。一种优化技术
    DS_HARD_DEADLOCK,    /* deadlock, no way out but ERROR */
    //存在死锁
    DS_BLOCKED_BY_AUTOVACUUM     /* no deadlock; queue blocked by autovacuum worker */
    //不存在死锁，只是被Autovacuum进程阻塞了
} DeadLockState;

```