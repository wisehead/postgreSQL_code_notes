#1.struct LOCK

```cpp
/*
 * Per-locked-object lock information:
 *
 * tag -- uniquely identifies the object being locked
 * grantMask -- bitmask for all lock types currently granted on this object.
 * waitMask -- bitmask for all lock types currently awaited on this object.
 * procLocks -- list of PROCLOCK objects for this lock.
 * waitProcs -- queue of processes waiting for this lock.
 * requested -- count of each lock type currently requested on the lock
 *      (includes requests already granted!!).
 * nRequested -- total requested locks of all types.
 * granted -- count of each lock type currently granted on the lock.
 * nGranted -- total granted locks of all types.
 *
 * Note: these counts count 1 for each backend.  Internally to a backend,
 * there may be multiple grabs on a particular lock, but this is not reflected
 * into shared memory.
 */
typedef struct LOCK
{
    /* hash key */
    LOCKTAG     tag;            /* unique identifier of lockable object */

    /* data */
    LOCKMASK    grantMask;      /* bitmask for lock types already granted */
    LOCKMASK    waitMask;       /* bitmask for lock types awaited */
    SHM_QUEUE   procLocks;      /* list of PROCLOCK objects assoc. with lock */
    PROC_QUEUE  waitProcs;      /* list of PGPROC objects waiting on lock */
    int         requested[MAX_LOCKMODES];       /* counts of requested locks */
    int         nRequested;     /* total of requested[] array */
    int         granted[MAX_LOCKMODES]; /* counts of granted locks */
    int         nGranted;       /* total of granted[] array */
} LOCK;

```