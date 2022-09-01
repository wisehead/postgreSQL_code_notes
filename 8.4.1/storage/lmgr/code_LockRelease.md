#1.LockRelease

```cpp
/*
 * LockRelease -- look up 'locktag' and release one 'lockmode' lock on it.
 *      Release a session lock if 'sessionLock' is true, else release a
 *      regular transaction lock.
 *
 * Side Effects: find any waiting processes that are now wakable,
 *      grant them their requested locks and awaken them.
 *      (We have to grant the lock here to avoid a race between
 *      the waking process and any new process to
 *      come along and request the lock.)
 */
bool
LockRelease(const LOCKTAG *locktag, LOCKMODE lockmode, bool sessionLock)
```