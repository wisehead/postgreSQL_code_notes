#1.struct LWLock

```cpp
typedef struct LWLock
{
    slock_t     mutex;          /* Protects LWLock and queue of PGPROCs */
    bool        releaseOK;      /* T if ok to release waiters */
    char        exclusive;      /* # of exclusive holders (0 or 1) */
    int         shared;         /* # of shared holders (0..MaxBackends) */
    PGPROC     *head;           /* head of list of waiting PGPROCs */
    PGPROC     *tail;           /* tail of list of waiting PGPROCs */
    /* tail is undefined when head is NULL */
} LWLock;
```