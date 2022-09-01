#1.struct DEADLOCK_INFO


```cpp
/*
 * Information saved about each edge in a detected deadlock cycle.  This
 * is used to print a diagnostic message upon failure.
 *
 * Note: because we want to examine this info after releasing the lock
 * manager's partition locks, we can't just store LOCK and PGPROC pointers;
 * we must extract out all the info we want to be able to print.
 */
typedef struct
{
    LOCKTAG     locktag;        /* ID of awaited lock object */
    LOCKMODE    lockmode;       /* type of lock we're waiting for */
    int         pid;            /* PID of blocked backend */
} DEADLOCK_INFO;

```