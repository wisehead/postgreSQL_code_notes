#1.LWLockWakeup

```
LWLockWakeup
--LWLockWaitListLock(lock);
--proclist_foreach_modify(iter, &lock->waiters, lwWaitLink)
----PGPROC	   *waiter = GetPGProcByNumber(iter.cur);
----proclist_delete(&lock->waiters, iter.cur, lwWaitLink);
----proclist_push_tail(&wakeup, iter.cur, lwWaitLink);
		/*
		 * Once we've woken up an exclusive lock, there's no point in waking
		 * up anybody else.
		 */
----if (waiter->lwWaitMode == LW_EXCLUSIVE)
------break;

--old_state = pg_atomic_read_u32(&lock->state);
--while (true)
		{
			desired_state = old_state;

			/* compute desired flags */

			if (new_release_ok)
				desired_state |= LW_FLAG_RELEASE_OK;
			else
				desired_state &= ~LW_FLAG_RELEASE_OK;

			if (proclist_is_empty(&wakeup))
				desired_state &= ~LW_FLAG_HAS_WAITERS;

			desired_state &= ~LW_FLAG_LOCKED;

			if (pg_atomic_compare_exchange_u32(&lock->state, &old_state,
											   desired_state))
				break;
--}

--proclist_foreach_modify(iter, &wakeup, lwWaitLink)
--{
----PGPROC	   *waiter = GetPGProcByNumber(iter.cur);     
----LOG_LWDEBUG("LWLockRelease", lock, "release waiter"); 
----proclist_delete(&wakeup, iter.cur, lwWaitLink);       
----pg_write_barrier();                                   
----waiter->lwWaiting = false;                            
----PGSemaphoreUnlock(waiter->sem);                       
--}
```