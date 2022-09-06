#1.struct PROCLOCK

```cpp

typedef struct PROCLOCK
{
    /* tag */
    PROCLOCKTAG tag;            /* unique identifier of proclock object */

    /* data */
    LOCKMASK    holdMask;       /* bitmask for lock types currently held */
    LOCKMASK    releaseMask;    /* bitmask for lock types to be released */
    SHM_QUEUE   lockLink;       /* list link in LOCK's list of proclocks */
    SHM_QUEUE   procLink;       /* list link in PGPROC's list of proclocks */
} PROCLOCK;

```