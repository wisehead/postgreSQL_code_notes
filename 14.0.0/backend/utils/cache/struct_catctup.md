#1.struct catctup

```cpp
typedef struct catctup
{
	int			ct_magic;		/* for identifying CatCTup entries */
#define CT_MAGIC   0x57261502

	uint32		hash_value;		/* hash value for this tuple's keys */

	/*
	 * Lookup keys for the entry. By-reference datums point into the tuple for
	 * positive cache entries, and are separately allocated for negative ones.
	 */
	Datum		keys[CATCACHE_MAXKEYS];

	/*
	 * Each tuple in a cache is a member of a dlist that stores the elements
	 * of its hash bucket.  We keep each dlist in LRU order to speed repeated
	 * lookups.
	 */
	dlist_node	cache_elem;		/* list member of per-bucket list */

	/*
	 * A tuple marked "dead" must not be returned by subsequent searches.
	 * However, it won't be physically deleted from the cache until its
	 * refcount goes to zero.  (If it's a member of a CatCList, the list's
	 * refcount must go to zero, too; also, remember to mark the list dead at
	 * the same time the tuple is marked.)
	 *
	 * A negative cache entry is an assertion that there is no tuple matching
	 * a particular key.  This is just as useful as a normal entry so far as
	 * avoiding catalog searches is concerned.  Management of positive and
	 * negative entries is identical.
	 */
	int			refcount;		/* number of active references */
	bool		dead;			/* dead but not yet removed? */
	bool		negative;		/* negative cache entry? */
	HeapTupleData tuple;		/* tuple management header */

	/*
	 * The tuple may also be a member of at most one CatCList.  (If a single
	 * catcache is list-searched with varying numbers of keys, we may have to
	 * make multiple entries for the same tuple because of this restriction.
	 * Currently, that's not expected to be common, so we accept the potential
	 * inefficiency.)
	 */
	struct catclist *c_list;	/* containing CatCList, or NULL if none */

	CatCache   *my_cache;		/* link to owning catcache */
	/* properly aligned tuple data follows, unless a negative entry */
} CatCTup;
```