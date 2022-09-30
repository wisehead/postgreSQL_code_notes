#1.struct SetExprState

```cpp
/* ----------------
 *		SetExprState node
 *
 * State for evaluating a potentially set-returning expression (like FuncExpr
 * or OpExpr).  In some cases, like some of the expressions in ROWS FROM(...)
 * the expression might not be a SRF, but nonetheless it uses the same
 * machinery as SRFs; it will be treated as a SRF returning a single row.
 * ----------------
 */
typedef struct SetExprState
{
	NodeTag		type;
	Expr	   *expr;			/* expression plan node */
	List	   *args;			/* ExprStates for argument expressions */

	/*
	 * In ROWS FROM, functions can be inlined, removing the FuncExpr normally
	 * inside.  In such a case this is the compiled expression (which cannot
	 * return a set), which'll be evaluated using regular ExecEvalExpr().
	 */
	ExprState  *elidedFuncState;

	/*
	 * Function manager's lookup info for the target function.  If func.fn_oid
	 * is InvalidOid, we haven't initialized it yet (nor any of the following
	 * fields, except funcReturnsSet).
	 */
	FmgrInfo	func;

	/*
	 * For a set-returning function (SRF) that returns a tuplestore, we keep
	 * the tuplestore here and dole out the result rows one at a time. The
	 * slot holds the row currently being returned.
	 */
	Tuplestorestate *funcResultStore;
	TupleTableSlot *funcResultSlot;

	/*
	 * In some cases we need to compute a tuple descriptor for the function's
	 * output.  If so, it's stored here.
	 */
	TupleDesc	funcResultDesc;
	bool		funcReturnsTuple;	/* valid when funcResultDesc isn't NULL */

	/*
	 * Remember whether the function is declared to return a set.  This is set
	 * by ExecInitExpr, and is valid even before the FmgrInfo is set up.
	 */
	bool		funcReturnsSet;

	/*
	 * setArgsValid is true when we are evaluating a set-returning function
	 * that uses value-per-call mode and we are in the middle of a call
	 * series; we want to pass the same argument values to the function again
	 * (and again, until it returns ExprEndResult).  This indicates that
	 * fcinfo_data already contains valid argument data.
	 */
	bool		setArgsValid;

	/*
	 * Flag to remember whether we have registered a shutdown callback for
	 * this SetExprState.  We do so only if funcResultStore or setArgsValid
	 * has been set at least once (since all the callback is for is to release
	 * the tuplestore or clear setArgsValid).
	 */
	bool		shutdown_reg;	/* a shutdown callback is registered */

	/*
	 * Call parameter structure for the function.  This has been initialized
	 * (by InitFunctionCallInfoData) if func.fn_oid is valid.  It also saves
	 * argument values between calls, when setArgsValid is true.
	 */
	FunctionCallInfo fcinfo;
} SetExprState;

```