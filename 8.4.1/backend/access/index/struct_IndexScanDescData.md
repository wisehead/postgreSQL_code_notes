#1.struct IndexScanDescData

```cpp

/*
 * We use the same IndexScanDescData structure for both amgettuple-based
 * and amgetbitmap-based index scans.  Some fields are only relevant in
 * amgettuple-based scans.
 */
typedef struct IndexScanDescData
{
    /* scan parameters */
    Relation    heapRelation;   /* heap relation descriptor, or NULL */
    Relation    indexRelation;  /* index relation descriptor */
    Snapshot    xs_snapshot;    /* snapshot to see */
    int         numberOfKeys;   /* number of scan keys */
    ScanKey     keyData;        /* array of scan key descriptors */

    /* signaling to index AM about killing index tuples */
    bool        kill_prior_tuple;       /* last-returned tuple is dead */
    bool        ignore_killed_tuples;   /* do not return killed entries */

    /* index access method's private state */
    void       *opaque;         /* access-method-specific info */

    /* xs_ctup/xs_cbuf/xs_recheck are valid after a successful index_getnext */
    HeapTupleData xs_ctup;      /* current heap tuple, if any */
    Buffer      xs_cbuf;        /* current heap buffer in scan, if any */
    /* NB: if xs_cbuf is not InvalidBuffer, we hold a pin on that buffer */
    bool        xs_recheck;     /* T means scan keys must be rechecked */

    /* state data for traversing HOT chains in index_getnext */
    bool        xs_hot_dead;    /* T if all members of HOT chain are dead */
    OffsetNumber xs_next_hot;   /* next member of HOT chain, if any */
    TransactionId xs_prev_xmax; /* previous HOT chain member's XMAX, if any */
} IndexScanDescData;
```