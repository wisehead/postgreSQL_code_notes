#1.struct TBMIterateResult

```cpp
/* Result structure for tbm_iterate */
typedef struct TBMIterateResult
{
	BlockNumber blockno;		/* page number containing tuples */
	int			ntuples;		/* -1 indicates lossy result */
	bool		recheck;		/* should the tuples be rechecked? */
	/* Note: recheck is always true if ntuples < 0 */
	OffsetNumber offsets[FLEXIBLE_ARRAY_MEMBER];
} TBMIterateResult;
```