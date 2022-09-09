#1.LWLockAttemptLock

```cpp
LWLockAttemptLock
--old_state = pg_atomic_read_u32(&lock->state);
--while (true)
----desired_state = old_state;
----if (mode == LW_EXCLUSIVE)
		{
			lock_free = (old_state & LW_LOCK_MASK) == 0;
			if (lock_free)
				desired_state += LW_VAL_EXCLUSIVE;
		}
----else
		{
			lock_free = (old_state & LW_VAL_EXCLUSIVE) == 0;
			if (lock_free)
				desired_state += LW_VAL_SHARED;
		}
----if (pg_atomic_compare_exchange_u32(&lock->state,
										   &old_state, desired_state))
------if (lock_free)f
--------return false;
------else
--------return true;	/* somebody else has the lock */
--//end while (true)
--pg_unreachable
```