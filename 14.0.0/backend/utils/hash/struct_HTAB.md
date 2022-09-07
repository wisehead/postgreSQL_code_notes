#1.struct HTAB

```cpp
/*
 * Top control structure for a hashtable --- in a shared table, each backend
 * has its own copy (OK since no fields change at runtime)
 */
struct HTAB
{
	HASHHDR    *hctl;			/* => shared control information */
	HASHSEGMENT *dir;			/* directory of segment starts */
	HashValueFunc hash;			/* hash function */
	HashCompareFunc match;		/* key comparison function */
	HashCopyFunc keycopy;		/* key copying function */
	HashAllocFunc alloc;		/* memory allocator */
	MemoryContext hcxt;			/* memory context if default allocator used */
	char	   *tabname;		/* table name (for error messages) */
	bool		isshared;		/* true if table is in shared memory */
	bool		isfixed;		/* if true, don't enlarge */

	/* freezing a shared table isn't allowed, so we can keep state here */
	bool		frozen;			/* true = no more inserts allowed */

	/* We keep local copies of these fixed values to reduce contention */
	Size		keysize;		/* hash key length in bytes */
	long		ssize;			/* segment size --- must be power of 2 */
	int			sshift;			/* segment shift = log2(ssize) */
};
```