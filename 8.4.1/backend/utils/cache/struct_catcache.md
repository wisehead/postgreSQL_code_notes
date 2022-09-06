#1.struct catcache

```cpp
/*
 *      struct catctup:         individual tuple in the cache.
 *      struct catclist:        list of tuples matching a partial key.
 *      struct catcache:        information for managing a cache.
 *      struct catcacheheader:  information for managing all the caches.
 */

typedef struct catcache
{
    int         id;             /* cache identifier --- see syscache.h */
    struct catcache *cc_next;   /* link to next catcache */
    const char *cc_relname;     /* name of relation the tuples come from */
    Oid         cc_reloid;      /* OID of relation the tuples come from */
    Oid         cc_indexoid;    /* OID of index matching cache keys */
    bool        cc_relisshared; /* is relation shared across databases? */
    TupleDesc   cc_tupdesc;     /* tuple descriptor (copied from reldesc) */
    int         cc_reloidattr;  /* AttrNumber of relation OID attr, or 0 */
    int         cc_ntup;        /* # of tuples currently in this cache */
    int         cc_nbuckets;    /* # of hash buckets in this cache */
    int         cc_nkeys;       /* # of keys (1..4) */
    int         cc_key[4];      /* AttrNumber of each key */
    PGFunction  cc_hashfunc[4]; /* hash function to use for each key */
    ScanKeyData cc_skey[4];     /* precomputed key info for heap scans */
    bool        cc_isname[4];   /* flag key columns that are NAMEs */
    Dllist      cc_lists;       /* list of CatCList structs */
#ifdef CATCACHE_STATS
    long        cc_searches;    /* total # searches against this cache */
    long        cc_hits;        /* # of matches against existing entry */
    long        cc_neg_hits;    /* # of matches against negative entry */
    long        cc_newloads;    /* # of successful loads of new entry */

    /*
     * cc_searches - (cc_hits + cc_neg_hits + cc_newloads) is number of failed
     * searches, each of which will result in loading a negative entry
     */
    long        cc_invals;      /* # of entries invalidated from cache */
    long        cc_lsearches;   /* total # list-searches */
    long        cc_lhits;       /* # of matches against existing lists */
#endif
    Dllist      cc_bucket[1];   /* hash buckets --- VARIABLE LENGTH ARRAY */
} CatCache;                     /* VARIABLE LENGTH STRUCT */

```