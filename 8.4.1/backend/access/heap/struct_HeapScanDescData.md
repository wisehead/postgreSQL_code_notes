#1.struct HeapScanDescData

```cpp
typedef struct HeapScanDescData
{
    /* scan parameters */
    Relation    rs_rd;          /* heap relation descriptor */
    Snapshot    rs_snapshot;    /* snapshot to see */
    int         rs_nkeys;       /* number of scan keys */
    ScanKey     rs_key;         /* array of scan key descriptors */
    bool        rs_bitmapscan;  /* true if this is really a bitmap scan */
    bool        rs_pageatatime; /* verify visibility page-at-a-time? */
    bool        rs_allow_strat; /* allow or disallow use of access strategy */
    bool        rs_allow_sync;  /* allow or disallow use of syncscan */

    /* state set up at initscan time */
    BlockNumber rs_nblocks;     /* number of blocks to scan */
    BlockNumber rs_startblock;  /* block # to start at */
    BufferAccessStrategy rs_strategy;   /* access strategy for reads */
    bool        rs_syncscan;    /* report location to syncscan logic? */

    /* scan current state */
    bool        rs_inited;      /* false = scan not init'd yet */
    HeapTupleData rs_ctup;      /* current tuple in scan, if any */
    BlockNumber rs_cblock;      /* current block # in scan, if any */
    Buffer      rs_cbuf;        /* current buffer in scan, if any */
    /* NB: if rs_cbuf is not InvalidBuffer, we hold a pin on that buffer */
    ItemPointerData rs_mctid;   /* marked scan position, if any */

    /* these fields only used in page-at-a-time mode and for bitmap scans */
    int         rs_cindex;      /* current tuple's index in vistuples */
    int         rs_mindex;      /* marked tuple's saved index */
    int         rs_ntuples;     /* number of visible tuples on page */
    OffsetNumber rs_vistuples[MaxHeapTuplesPerPage];    /* their offsets */
} HeapScanDescData;
```