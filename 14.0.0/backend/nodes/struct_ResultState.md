#1.struct ResultState

```cpp
/* ----------------
 *	 ResultState information
 * ----------------
 */
typedef struct ResultState
{
	PlanState	ps;				/* its first field is NodeTag */
	ExprState  *resconstantqual;
	bool		rs_done;		/* are we done? */
	bool		rs_checkqual;	/* do we need to check the qual? */
} ResultState;
```