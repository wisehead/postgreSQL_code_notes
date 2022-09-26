#1.enum ExprEvalOp

```cpp
/*
 * Discriminator for ExprEvalSteps.
 *
 * Identifies the operation to be executed and which member in the
 * ExprEvalStep->d union is valid.
 *
 * The order of entries needs to be kept in sync with the dispatch_table[]
 * array in execExprInterp.c:ExecInterpExpr().
 */
typedef enum ExprEvalOp
{
	/* entire expression has been evaluated completely, return */
	EEOP_DONE,

	/* apply slot_getsomeattrs on corresponding tuple slot */
	EEOP_INNER_FETCHSOME,
	EEOP_OUTER_FETCHSOME,
	EEOP_SCAN_FETCHSOME,

	/* compute non-system Var value */
	EEOP_INNER_VAR,
	EEOP_OUTER_VAR,
	EEOP_SCAN_VAR,

	/* compute system Var value */
	EEOP_INNER_SYSVAR,
	EEOP_OUTER_SYSVAR,
	EEOP_SCAN_SYSVAR,

	/* compute wholerow Var */
	EEOP_WHOLEROW,

	/*
	 * Compute non-system Var value, assign it into ExprState's resultslot.
	 * These are not used if a CheckVarSlotCompatibility() check would be
	 * needed.
	 */
	EEOP_ASSIGN_INNER_VAR,
	EEOP_ASSIGN_OUTER_VAR,
	EEOP_ASSIGN_SCAN_VAR,

	/* assign ExprState's resvalue/resnull to a column of its resultslot */
	EEOP_ASSIGN_TMP,
	/* ditto, applying MakeExpandedObjectReadOnly() */
	EEOP_ASSIGN_TMP_MAKE_RO,

	/* evaluate Const value */
	EEOP_CONST,

	/*
	 * Evaluate function call (including OpExprs etc).  For speed, we
	 * distinguish in the opcode whether the function is strict and/or
	 * requires usage stats tracking.
	 */
	EEOP_FUNCEXPR,
	EEOP_FUNCEXPR_STRICT,
	EEOP_FUNCEXPR_FUSAGE,
	EEOP_FUNCEXPR_STRICT_FUSAGE,

	/*
	 * Evaluate boolean AND expression, one step per subexpression. FIRST/LAST
	 * subexpressions are special-cased for performance.  Since AND always has
	 * at least two subexpressions, FIRST and LAST never apply to the same
	 * subexpression.
	 */
	EEOP_BOOL_AND_STEP_FIRST,
	EEOP_BOOL_AND_STEP,
	EEOP_BOOL_AND_STEP_LAST,

	/* similarly for boolean OR expression */
	EEOP_BOOL_OR_STEP_FIRST,
	EEOP_BOOL_OR_STEP,
	EEOP_BOOL_OR_STEP_LAST,

	/* evaluate boolean NOT expression */
	EEOP_BOOL_NOT_STEP,

	/* simplified version of BOOL_AND_STEP for use by ExecQual() */
	EEOP_QUAL,

	/* unconditional jump to another step */
	EEOP_JUMP,

	/* conditional jumps based on current result value */
	EEOP_JUMP_IF_NULL,
	EEOP_JUMP_IF_NOT_NULL,
	EEOP_JUMP_IF_NOT_TRUE,

	/* perform NULL tests for scalar values */
	EEOP_NULLTEST_ISNULL,
	EEOP_NULLTEST_ISNOTNULL,

	/* perform NULL tests for row values */
	EEOP_NULLTEST_ROWISNULL,
	EEOP_NULLTEST_ROWISNOTNULL,

	/* evaluate a BooleanTest expression */
	EEOP_BOOLTEST_IS_TRUE,
	EEOP_BOOLTEST_IS_NOT_TRUE,
	EEOP_BOOLTEST_IS_FALSE,
	EEOP_BOOLTEST_IS_NOT_FALSE,

	/* evaluate PARAM_EXEC/EXTERN parameters */
	EEOP_PARAM_EXEC,
	EEOP_PARAM_EXTERN,
	EEOP_PARAM_CALLBACK,

	/* return CaseTestExpr value */
	EEOP_CASE_TESTVAL,

	/* apply MakeExpandedObjectReadOnly() to target value */
	EEOP_MAKE_READONLY,

	/* evaluate assorted special-purpose expression types */
	EEOP_IOCOERCE,
	EEOP_DISTINCT,
	EEOP_NOT_DISTINCT,
	EEOP_NULLIF,
	EEOP_SQLVALUEFUNCTION,
	EEOP_CURRENTOFEXPR,
	EEOP_NEXTVALUEEXPR,
	EEOP_ARRAYEXPR,
	EEOP_ARRAYCOERCE,
	EEOP_ROW,

	/*
	 * Compare two individual elements of each of two compared ROW()
	 * expressions.  Skip to ROWCOMPARE_FINAL if elements are not equal.
	 */
	EEOP_ROWCOMPARE_STEP,

	/* evaluate boolean value based on previous ROWCOMPARE_STEP operations */
	EEOP_ROWCOMPARE_FINAL,

	/* evaluate GREATEST() or LEAST() */
	EEOP_MINMAX,

	/* evaluate FieldSelect expression */
	EEOP_FIELDSELECT,

	/*
	 * Deform tuple before evaluating new values for individual fields in a
	 * FieldStore expression.
	 */
	EEOP_FIELDSTORE_DEFORM,

	/*
	 * Form the new tuple for a FieldStore expression.  Individual fields will
	 * have been evaluated into columns of the tuple deformed by the preceding
	 * DEFORM step.
	 */
	EEOP_FIELDSTORE_FORM,

	/* Process container subscripts; possibly short-circuit result to NULL */
	EEOP_SBSREF_SUBSCRIPTS,

	/*
	 * Compute old container element/slice when a SubscriptingRef assignment
	 * expression contains SubscriptingRef/FieldStore subexpressions. Value is
	 * accessed using the CaseTest mechanism.
	 */
	EEOP_SBSREF_OLD,

	/* compute new value for SubscriptingRef assignment expression */
	EEOP_SBSREF_ASSIGN,

	/* compute element/slice for SubscriptingRef fetch expression */
	EEOP_SBSREF_FETCH,

	/* evaluate value for CoerceToDomainValue */
	EEOP_DOMAIN_TESTVAL,

	/* evaluate a domain's NOT NULL constraint */
	EEOP_DOMAIN_NOTNULL,

	/* evaluate a single domain CHECK constraint */
	EEOP_DOMAIN_CHECK,

	/* evaluate assorted special-purpose expression types */
	EEOP_CONVERT_ROWTYPE,
	EEOP_SCALARARRAYOP,
	EEOP_HASHED_SCALARARRAYOP,
	EEOP_XMLEXPR,
	EEOP_AGGREF,
	EEOP_GROUPING_FUNC,
	EEOP_WINDOW_FUNC,
	EEOP_SUBPLAN,

	/* aggregation related nodes */
	EEOP_AGG_STRICT_DESERIALIZE,
	EEOP_AGG_DESERIALIZE,
	EEOP_AGG_STRICT_INPUT_CHECK_ARGS,
	EEOP_AGG_STRICT_INPUT_CHECK_NULLS,
	EEOP_AGG_PLAIN_PERGROUP_NULLCHECK,
	EEOP_AGG_PLAIN_TRANS_INIT_STRICT_BYVAL,
	EEOP_AGG_PLAIN_TRANS_STRICT_BYVAL,
	EEOP_AGG_PLAIN_TRANS_BYVAL,
	EEOP_AGG_PLAIN_TRANS_INIT_STRICT_BYREF,
	EEOP_AGG_PLAIN_TRANS_STRICT_BYREF,
	EEOP_AGG_PLAIN_TRANS_BYREF,
	EEOP_AGG_ORDERED_TRANS_DATUM,
	EEOP_AGG_ORDERED_TRANS_TUPLE,

	/* non-existent operation, used e.g. to check array lengths */
	EEOP_LAST
} ExprEvalOp;


```