#1.enum LockAcquireResult

```cpp
/* Result codes for LockAcquire() */
typedef enum
{
    LOCKACQUIRE_NOT_AVAIL,      /* lock not available, and dontWait=true */
    LOCKACQUIRE_OK,             /* lock successfully acquired */
    LOCKACQUIRE_ALREADY_HELD    /* incremented count for lock already held */
} LockAcquireResult;

```