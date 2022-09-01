#1.LockAcquire

```cpp
/*
 * LockAcquire -- Check for lock conflicts, sleep if conflict found,
 *      set lock if/when no conflicts.
 *
 * Inputs:
 *  locktag: unique identifier for the lockable object
 *  lockmode: lock mode to acquire
 *  sessionLock: if true, acquire lock for session not current transaction
 *  dontWait: if true, don't wait to acquire lock
 *
 * Returns one of:
 *      LOCKACQUIRE_NOT_AVAIL       lock not available, and dontWait=true
 *      LOCKACQUIRE_OK              lock successfully acquired
 *      LOCKACQUIRE_ALREADY_HELD    incremented count for lock already held
 *
 * In the normal case where dontWait=false and the caller doesn't need to
 * distinguish a freshly acquired lock from one already taken earlier in
 * this same transaction, there is no need to examine the return value.
 *
 * Side Effects: The lock is acquired and recorded in lock tables.
 *
 * NOTE: if we wait for the lock, there is no way to abort the wait
 * short of aborting the transaction.
 */
LockAcquireResult
LockAcquire(const LOCKTAG *locktag,
            LOCKMODE lockmode,
            bool sessionLock,
            bool dontWait)

```