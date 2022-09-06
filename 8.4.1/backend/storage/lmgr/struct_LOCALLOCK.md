#1.struct LOCALLOCK

```cpp
typedef struct LOCALLOCK
{
    /* tag */
    LOCALLOCKTAG tag;           /* unique identifier of locallock entry */

    /* data */
    LOCK       *lock;           /* associated LOCK object in shared mem */
    PROCLOCK   *proclock;       /* associated PROCLOCK object in shmem */
    uint32      hashcode;       /* copy of LOCKTAG's hash value */
    int64       nLocks;         /* total number of times lock is held */
    int         numLockOwners;  /* # of relevant ResourceOwners */
    int         maxLockOwners;  /* allocated size of array */
    LOCALLOCKOWNER *lockOwners; /* dynamically resizable array */
} LOCALLOCK;

```