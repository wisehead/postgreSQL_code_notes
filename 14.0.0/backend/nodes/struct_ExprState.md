#1.struct ExprState

```cpp
typedef struct ExprState
{
	NodeTag		tag;

	uint8		flags;			/* bitmask of EEO_FLAG_* bits, see above */

	/*
	 * Storage for result value of a scalar expression, or for individual
	 * column results within expressions built by ExecBuildProjectionInfo().
	 */
#define FIELDNO_EXPRSTATE_RESNULL 2
	bool		resnull;
#define FIELDNO_EXPRSTATE_RESVALUE 3
	Datum		resvalue;

	/*
	 * If projecting a tuple result, this slot holds the result; else NULL.
	 */
#define FIELDNO_EXPRSTATE_RESULTSLOT 4
	TupleTableSlot *resultslot;

	/*
	 * Instructions to compute expression's return value.
	 */
	struct ExprEvalStep *steps;

	/*
	 * Function that actually evaluates the expression.  This can be set to
	 * different values depending on the complexity of the expression.
	 */
	ExprStateEvalFunc evalfunc;

	/* original expression tree, for debugging only */
	Expr	   *expr;

	/* private state for an evalfunc */
	void	   *evalfunc_private;

	/*
	 * XXX: following fields only needed during "compilation" (ExecInitExpr);
	 * could be thrown away afterwards.
	 */

	int			steps_len;		/* number of steps currently */
	int			steps_alloc;	/* allocated length of steps array */

#define FIELDNO_EXPRSTATE_PARENT 11
	struct PlanState *parent;	/* parent PlanState node, if any */
	ParamListInfo ext_params;	/* for compiling PARAM_EXTERN nodes */

	Datum	   *innermost_caseval;
	bool	   *innermost_casenull;

	Datum	   *innermost_domainval;
	bool	   *innermost_domainnull;
} ExprState;

```

