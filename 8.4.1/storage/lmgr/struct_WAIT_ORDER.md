#1.struct WAIT_ORDER

```cpp
/* One potential reordering of a lock's wait queue */
typedef struct
{
    LOCK       *lock;           /* the lock whose wait queue is described */
    PGPROC    **procs;          /* array of PGPROC *'s in new wait order */
    int         nProcs;
} WAIT_ORDER;
```