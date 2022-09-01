#1.enum DeadLockState

```cpp
/* Deadlock states identified by DeadLockCheck() */
typedef enum
{
    DS_NOT_YET_CHECKED,         /* no deadlock check has run yet */
    DS_NO_DEADLOCK,             /* no deadlock detected */
    DS_SOFT_DEADLOCK,           /* deadlock avoided by queue rearrangement */
    DS_HARD_DEADLOCK,           /* deadlock, no way out but ERROR */
    DS_BLOCKED_BY_AUTOVACUUM    /* no deadlock; queue blocked by autovacuum
                                 * worker */
} DeadLockState;
```