#1.struct CteScanState

```cpp
/* ----------------
 *	 CteScanState information
 *
 *		CteScan nodes are used to scan a CommonTableExpr query.
 *
 * Multiple CteScan nodes can read out from the same CTE query.  We use
 * a tuplestore to hold rows that have been read from the CTE query but
 * not yet consumed by all readers.
 * ----------------
 */
typedef struct CteScanState
{
	ScanState	ss;				/* its first field is NodeTag */
	int			eflags;			/* capability flags to pass to tuplestore */
	int			readptr;		/* index of my tuplestore read pointer */
	PlanState  *cteplanstate;	/* PlanState for the CTE query itself */
	/* Link to the "leader" CteScanState (possibly this same node) */
	struct CteScanState *leader;
	/* The remaining fields are only valid in the "leader" CteScanState */
	Tuplestorestate *cte_table; /* rows already read from the CTE query */
	bool		eof_cte;		/* reached end of CTE query? */
} CteScanState;
```