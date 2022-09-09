#1.LWLockWaitListUnlock

```cpp
LWLockWaitListUnlock
--old_state = pg_atomic_fetch_and_u32(&lock->state, ~LW_FLAG_LOCKED);
```