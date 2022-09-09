#1.LWLockRelease

```cpp
LWLockRelease
--for (i = num_held_lwlocks; --i >= 0;)
		if (lock == held_lwlocks[i].lock)
			break;
--for (; i < num_held_lwlocks; i++)//挪动数组，无空洞
		held_lwlocks[i] = held_lwlocks[i + 1];
--if (mode == LW_EXCLUSIVE)
		oldstate = pg_atomic_sub_fetch_u32(&lock->state, LW_VAL_EXCLUSIVE);
	else
		oldstate = pg_atomic_sub_fetch_u32(&lock->state, LW_VAL_SHARED);
--if ((oldstate & (LW_FLAG_HAS_WAITERS | LW_FLAG_RELEASE_OK)) ==
		(LW_FLAG_HAS_WAITERS | LW_FLAG_RELEASE_OK) &&
		(oldstate & LW_LOCK_MASK) == 0)
		check_waiters = true;
	else
		check_waiters = false;
--if (check_waiters)
----LWLockWakeup(lock);
--RESUME_INTERRUPTS();

```