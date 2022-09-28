#1.struct BitmapHeapScanState

```cpp
/* ----------------
 *	 BitmapHeapScanState information
 *
 *		bitmapqualorig	   execution state for bitmapqualorig expressions
 *		tbm				   bitmap obtained from child index scan(s)
 *		tbmiterator		   iterator for scanning current pages
 *		tbmres			   current-page data
 *		can_skip_fetch	   can we potentially skip tuple fetches in this scan?
 *		return_empty_tuples number of empty tuples to return
 *		vmbuffer		   buffer for visibility-map lookups
 *		pvmbuffer		   ditto, for prefetched pages
 *		exact_pages		   total number of exact pages retrieved
 *		lossy_pages		   total number of lossy pages retrieved
 *		prefetch_iterator  iterator for prefetching ahead of current page
 *		prefetch_pages	   # pages prefetch iterator is ahead of current
 *		prefetch_target    current target prefetch distance
 *		prefetch_maximum   maximum value for prefetch_target
 *		pscan_len		   size of the shared memory for parallel bitmap
 *		initialized		   is node is ready to iterate
 *		shared_tbmiterator	   shared iterator
 *		shared_prefetch_iterator shared iterator for prefetching
 *		pstate			   shared state for parallel bitmap scan
 * ----------------
 */
typedef struct BitmapHeapScanState
{
	ScanState	ss;				/* its first field is NodeTag */
	ExprState  *bitmapqualorig;
	TIDBitmap  *tbm;
	TBMIterator *tbmiterator;
	TBMIterateResult *tbmres;
	bool		can_skip_fetch;
	int			return_empty_tuples;
	Buffer		vmbuffer;
	Buffer		pvmbuffer;
	long		exact_pages;
	long		lossy_pages;
	TBMIterator *prefetch_iterator;
	int			prefetch_pages;
	int			prefetch_target;
	int			prefetch_maximum;
	Size		pscan_len;
	bool		initialized;
	TBMSharedIterator *shared_tbmiterator;
	TBMSharedIterator *shared_prefetch_iterator;
	ParallelBitmapHeapState *pstate;
} BitmapHeapScanState;
```