#1.struct FuncExpr

```cpp
/*
 * FuncExpr - expression node for a function call
 */
typedef struct FuncExpr
{
	Expr		xpr;
	Oid			funcid;			/* PG_PROC OID of the function */
	Oid			funcresulttype; /* PG_TYPE OID of result value */
	bool		funcretset;		/* true if function returns set */
	bool		funcvariadic;	/* true if variadic arguments have been
								 * combined into an array last argument */
	CoercionForm funcformat;	/* how to display this function call */
	Oid			funccollid;		/* OID of collation of result */
	Oid			inputcollid;	/* OID of collation that function should use */
	List	   *args;			/* arguments to the function */
	int			location;		/* token location, or -1 if unknown */
} FuncExpr;
```