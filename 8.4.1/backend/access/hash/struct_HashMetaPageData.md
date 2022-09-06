#1.struct HashMetaPageData

```cpp
typedef struct HashMetaPageData
{
    uint32      hashm_magic;    /* magic no. for hash tables */
    uint32      hashm_version;  /* version ID */
    double      hashm_ntuples;  /* number of tuples stored in the table */
    uint16      hashm_ffactor;  /* target fill factor (tuples/bucket) */
    uint16      hashm_bsize;    /* index page size (bytes) */
    uint16      hashm_bmsize;   /* bitmap array size (bytes) - must be a power
                                 * of 2 */
    uint16      hashm_bmshift;  /* log2(bitmap array size in BITS) */
    uint32      hashm_maxbucket;    /* ID of maximum bucket in use */
    uint32      hashm_highmask; /* mask to modulo into entire table */
    uint32      hashm_lowmask;  /* mask to modulo into lower half of table */
    uint32      hashm_ovflpoint;/* splitpoint from which ovflpgs being
                                 * allocated */
    uint32      hashm_firstfree;    /* lowest-number free ovflpage (bit#) */
    uint32      hashm_nmaps;    /* number of bitmap pages */
    RegProcedure hashm_procid;  /* hash procedure id from pg_proc */
    uint32      hashm_spares[HASH_MAX_SPLITPOINTS];     /* spare pages before
                                                         * each splitpoint */
    BlockNumber hashm_mapp[HASH_MAX_BITMAPS];   /* blknos of ovfl bitmaps */
} HashMetaPageData;

typedef HashMetaPageData *HashMetaPage;

```