#1.struct catclist

```cpp
/*
 * A CatCList describes the result of a partial search, ie, a search using
 * only the first K key columns of an N-key cache.  We store the keys used
 * into the keys attribute to represent the stored key set.  The CatCList
 * object contains links to cache entries for all the table rows satisfying
 * the partial key.  (Note: none of these will be negative cache entries.)
 *
 * A CatCList is only a member of a per-cache list; we do not currently
 * divide them into hash buckets.
 *
 * A list marked "dead" must not be returned by subsequent searches.
 * However, it won't be physically deleted from the cache until its
 * refcount goes to zero.  (A list should be marked dead if any of its
 * member entries are dead.)
 *
 * If "ordered" is true then the member tuples appear in the order of the
 * cache's underlying index.  This will be true in normal operation, but
 * might not be true during bootstrap or recovery operations. (namespace.c
 * is able to save some cycles when it is true.)
 */
typedef struct catclist
{
	int			cl_magic;		/* for identifying CatCList entries */
#define CL_MAGIC   0x52765103

	uint32		hash_value;		/* hash value for lookup keys */

	dlist_node	cache_elem;		/* list member of per-catcache list */

	/*
	 * Lookup keys for the entry, with the first nkeys elements being valid.
	 * All by-reference are separately allocated.
	 */
	Datum		keys[CATCACHE_MAXKEYS];

	int			refcount;		/* number of active references */
	bool		dead;			/* dead but not yet removed? */
	bool		ordered;		/* members listed in index order? */
	short		nkeys;			/* number of lookup keys specified */
	int			n_members;		/* number of member tuples */
	CatCache   *my_cache;		/* link to owning catcache */
	CatCTup    *members[FLEXIBLE_ARRAY_MEMBER]; /* members */
} CatCList;


```