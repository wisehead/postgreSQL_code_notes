#1.LWLockAttemptLock

```cpp
LWLockAttemptLock
--old_state = pg_atomic_read_u32(&lock->state);
```