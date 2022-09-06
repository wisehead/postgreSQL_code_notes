#1.struct ResourceArray

```cpp
/*
 * ResourceArray is a common structure for storing all types of resource IDs.
 *
 * We manage small sets of resource IDs by keeping them in a simple array:
 * itemsarr[k] holds an ID, for 0 <= k < nitems <= maxitems = capacity.
 *
 * If a set grows large, we switch over to using open-addressing hashing.
 * Then, itemsarr[] is a hash table of "capacity" slots, with each
 * slot holding either an ID or "invalidval".  nitems is the number of valid
 * items present; if it would exceed maxitems, we enlarge the array and
 * re-hash.  In this mode, maxitems should be rather less than capacity so
 * that we don't waste too much time searching for empty slots.
 *
 * In either mode, lastidx remembers the location of the last item inserted
 * or returned by GetAny; this speeds up searches in ResourceArrayRemove.
 */
typedef struct ResourceArray
{
	Datum	   *itemsarr;		/* buffer for storing values */
	Datum		invalidval;		/* value that is considered invalid */
	uint32		capacity;		/* allocated length of itemsarr[] */
	uint32		nitems;			/* how many items are stored in items array */
	uint32		maxitems;		/* current limit on nitems before enlarging */
	uint32		lastidx;		/* index of last item returned by GetAny */
} ResourceArray;
```