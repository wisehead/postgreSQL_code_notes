#1.struct ScalarArrayOpExpr

```cpp
/*
 * ScalarArrayOpExpr - expression node for "scalar op ANY/ALL (array)"
 *
 * The operator must yield boolean.  It is applied to the left operand
 * and each element of the righthand array, and the results are combined
 * with OR or AND (for ANY or ALL respectively).  The node representation
 * is almost the same as for the underlying operator, but we need a useOr
 * flag to remember whether it's ANY or ALL, and we don't have to store
 * the result type (or the collation) because it must be boolean.
 *
 * A ScalarArrayOpExpr with a valid hashfuncid is evaluated during execution
 * by building a hash table containing the Const values from the rhs arg.
 * This table is probed during expression evaluation.  Only useOr=true
 * ScalarArrayOpExpr with Const arrays on the rhs can have the hashfuncid
 * field set. See convert_saop_to_hashed_saop().
 */
typedef struct ScalarArrayOpExpr
{
	Expr		xpr;
	Oid			opno;			/* PG_OPERATOR OID of the operator */
	Oid			opfuncid;		/* PG_PROC OID of comparison function */
	Oid			hashfuncid;		/* PG_PROC OID of hash func or InvalidOid */
	bool		useOr;			/* true for ANY, false for ALL */
	Oid			inputcollid;	/* OID of collation that operator should use */
	List	   *args;			/* the scalar and array operands */
	int			location;		/* token location, or -1 if unknown */
} ScalarArrayOpExpr;

```