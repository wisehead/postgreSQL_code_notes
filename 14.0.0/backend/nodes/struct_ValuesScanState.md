#1.struct ValuesScanState

```cpp
/* ----------------
 *	 ValuesScanState information
 *
 *		ValuesScan nodes are used to scan the results of a VALUES list
 *
 *		rowcontext			per-expression-list context
 *		exprlists			array of expression lists being evaluated
 *		exprstatelists		array of expression state lists, for SubPlans only
 *		array_len			size of above arrays
 *		curr_idx			current array index (0-based)
 *
 *	Note: ss.ps.ps_ExprContext is used to evaluate any qual or projection
 *	expressions attached to the node.  We create a second ExprContext,
 *	rowcontext, in which to build the executor expression state for each
 *	Values sublist.  Resetting this context lets us get rid of expression
 *	state for each row, avoiding major memory leakage over a long values list.
 *	However, that doesn't work for sublists containing SubPlans, because a
 *	SubPlan has to be connected up to the outer plan tree to work properly.
 *	Therefore, for only those sublists containing SubPlans, we do expression
 *	state construction at executor start, and store those pointers in
 *	exprstatelists[].  NULL entries in that array correspond to simple
 *	subexpressions that are handled as described above.
 * ----------------
 */
typedef struct ValuesScanState
{
	ScanState	ss;				/* its first field is NodeTag */
	ExprContext *rowcontext;
	List	  **exprlists;
	List	  **exprstatelists;
	int			array_len;
	int			curr_idx;
} ValuesScanState;
```