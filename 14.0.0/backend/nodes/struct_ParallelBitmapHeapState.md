#1.struct ParallelBitmapHeapState

```cpp
/* ----------------
 *	 ParallelBitmapHeapState information
 *		tbmiterator				iterator for scanning current pages
 *		prefetch_iterator		iterator for prefetching ahead of current page
 *		mutex					mutual exclusion for the prefetching variable
 *								and state
 *		prefetch_pages			# pages prefetch iterator is ahead of current
 *		prefetch_target			current target prefetch distance
 *		state					current state of the TIDBitmap
 *		cv						conditional wait variable
 *		phs_snapshot_data		snapshot data shared to workers
 * ----------------
 */
typedef struct ParallelBitmapHeapState
{
	dsa_pointer tbmiterator;
	dsa_pointer prefetch_iterator;
	slock_t		mutex;
	int			prefetch_pages;
	int			prefetch_target;
	SharedBitmapState state;
	ConditionVariable cv;
	char		phs_snapshot_data[FLEXIBLE_ARRAY_MEMBER];
} ParallelBitmapHeapState;
```