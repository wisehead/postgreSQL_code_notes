#1.LWLockWaitListLock

```
LWLockWaitListLock
--while (true)
----old_state = pg_atomic_fetch_or_u32(&lock->state, LW_FLAG_LOCKED);
----if (!(old_state & LW_FLAG_LOCKED))
------break;	
----init_local_spin_delay(&delayStatus);
----while (old_state & LW_FLAG_LOCKED)
------perform_spin_delay(&delayStatus);
------old_state = pg_atomic_read_u32(&lock->state);
----finish_spin_delay(&delayStatus);
--//end while (true)
```