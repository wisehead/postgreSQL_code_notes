#1.struct ForeignScanState

```cpp
/* ----------------
 *	 ForeignScanState information
 *
 *		ForeignScan nodes are used to scan foreign-data tables.
 * ----------------
 */
typedef struct ForeignScanState
{
	ScanState	ss;				/* its first field is NodeTag */
	ExprState  *fdw_recheck_quals;	/* original quals not in ss.ps.qual */
	Size		pscan_len;		/* size of parallel coordination information */
	ResultRelInfo *resultRelInfo;	/* result rel info, if UPDATE or DELETE */
	/* use struct pointer to avoid including fdwapi.h here */
	struct FdwRoutine *fdwroutine;
	void	   *fdw_state;		/* foreign-data wrapper can keep state here */
} ForeignScanState;
```