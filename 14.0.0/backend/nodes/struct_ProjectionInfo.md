#1.struct ProjectionInfo

```cpp
/* ----------------
 *		ProjectionInfo node information
 *
 *		This is all the information needed to perform projections ---
 *		that is, form new tuples by evaluation of targetlist expressions.
 *		Nodes which need to do projections create one of these.
 *
 *		The target tuple slot is kept in ProjectionInfo->pi_state.resultslot.
 *		ExecProject() evaluates the tlist, forms a tuple, and stores it
 *		in the given slot.  Note that the result will be a "virtual" tuple
 *		unless ExecMaterializeSlot() is then called to force it to be
 *		converted to a physical tuple.  The slot must have a tupledesc
 *		that matches the output of the tlist!
 * ----------------
 */
typedef struct ProjectionInfo
{
	NodeTag		type;
	/* instructions to evaluate projection */
	ExprState	pi_state;
	/* expression context in which to evaluate expression */
	ExprContext *pi_exprContext;
} ProjectionInfo;
```