#1.struct OpExpr

```cpp
/*
 * OpExpr - expression node for an operator invocation
 *
 * Semantically, this is essentially the same as a function call.
 *
 * Note that opfuncid is not necessarily filled in immediately on creation
 * of the node.  The planner makes sure it is valid before passing the node
 * tree to the executor, but during parsing/planning opfuncid can be 0.
 */
typedef struct OpExpr
{
	Expr		xpr;
	Oid			opno;			/* PG_OPERATOR OID of the operator */
	Oid			opfuncid;		/* PG_PROC OID of underlying function */
	Oid			opresulttype;	/* PG_TYPE OID of result value */
	bool		opretset;		/* true if operator returns set */
	Oid			opcollid;		/* OID of collation of result */
	Oid			inputcollid;	/* OID of collation that operator should use */
	List	   *args;			/* arguments to the operator (1 or 2) */
	int			location;		/* token location, or -1 if unknown */
} OpExpr;

```