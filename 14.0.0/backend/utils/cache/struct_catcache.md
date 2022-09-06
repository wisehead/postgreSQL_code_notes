#1.struct catcache

```cpp

typedef struct catcache
{
	int			id;				/* cache identifier --- see syscache.h */
	int			cc_nbuckets;	/* # of hash buckets in this cache */
	TupleDesc	cc_tupdesc;		/* tuple descriptor (copied from reldesc) */
	dlist_head *cc_bucket;		/* hash buckets */
	CCHashFN	cc_hashfunc[CATCACHE_MAXKEYS];	/* hash function for each key */
	CCFastEqualFN cc_fastequal[CATCACHE_MAXKEYS];	/* fast equal function for
													 * each key */
	int			cc_keyno[CATCACHE_MAXKEYS]; /* AttrNumber of each key */
	dlist_head	cc_lists;		/* list of CatCList structs */
	int			cc_ntup;		/* # of tuples currently in this cache */
	int			cc_nkeys;		/* # of keys (1..CATCACHE_MAXKEYS) */
	const char *cc_relname;		/* name of relation the tuples come from */
	Oid			cc_reloid;		/* OID of relation the tuples come from */
	Oid			cc_indexoid;	/* OID of index matching cache keys */
	bool		cc_relisshared; /* is relation shared across databases? */
	slist_node	cc_next;		/* list link */
	ScanKeyData cc_skey[CATCACHE_MAXKEYS];	/* precomputed key info for heap
											 * scans */

	/*
	 * Keep these at the end, so that compiling catcache.c with CATCACHE_STATS
	 * doesn't break ABI for other modules
	 */
#ifdef CATCACHE_STATS
	long		cc_searches;	/* total # searches against this cache */
	long		cc_hits;		/* # of matches against existing entry */
	long		cc_neg_hits;	/* # of matches against negative entry */
	long		cc_newloads;	/* # of successful loads of new entry */

	/*
	 * cc_searches - (cc_hits + cc_neg_hits + cc_newloads) is number of failed
	 * searches, each of which will result in loading a negative entry
	 */
	long		cc_invals;		/* # of entries invalidated from cache */
	long		cc_lsearches;	/* total # list-searches */
	long		cc_lhits;		/* # of matches against existing lists */
#endif
} CatCache;
```