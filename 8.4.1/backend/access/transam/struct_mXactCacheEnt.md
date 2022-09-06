#1.struct mXactCacheEnt

```cpp
/*
 * Definitions for the backend-local MultiXactId cache.
 *
 * We use this cache to store known MultiXacts, so we don't need to go to
 * SLRU areas everytime.
 *
 * The cache lasts for the duration of a single transaction, the rationale
 * for this being that most entries will contain our own TransactionId and
 * so they will be uninteresting by the time our next transaction starts.
 * (XXX not clear that this is correct --- other members of the MultiXact
 * could hang around longer than we did.  However, it's not clear what a
 * better policy for flushing old cache entries would be.)
 *
 * We allocate the cache entries in a memory context that is deleted at
 * transaction end, so we don't need to do retail freeing of entries.
 */
typedef struct mXactCacheEnt
{
    struct mXactCacheEnt *next;
    MultiXactId multi;
    int         nxids;
    TransactionId xids[1];      /* VARIABLE LENGTH ARRAY */
} mXactCacheEnt;
```