#1.struct SortState

```cpp
/* ----------------
 *	 SortState information
 * ----------------
 */
typedef struct SortState
{
	ScanState	ss;				/* its first field is NodeTag */
	bool		randomAccess;	/* need random access to sort output? */
	bool		bounded;		/* is the result set bounded? */
	int64		bound;			/* if bounded, how many tuples are needed */
	bool		sort_Done;		/* sort completed yet? */
	bool		bounded_Done;	/* value of bounded we did the sort with */
	int64		bound_Done;		/* value of bound we did the sort with */
	void	   *tuplesortstate; /* private state of tuplesort.c */
	bool		am_worker;		/* are we a worker? */
	SharedSortInfo *shared_info;	/* one entry per worker */
} SortState;
```