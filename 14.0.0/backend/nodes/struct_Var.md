#1.struct Var

```cpp
typedef struct Var
{
	Expr		xpr;
	Index		varno;			/* index of this var's relation in the range
								 * table, or INNER_VAR/OUTER_VAR/INDEX_VAR */
	AttrNumber	varattno;		/* attribute number of this var, or zero for
								 * all attrs ("whole-row Var") */
	Oid			vartype;		/* pg_type OID for the type of this var */
	int32		vartypmod;		/* pg_attribute typmod value */
	Oid			varcollid;		/* OID of collation, or InvalidOid if none */
	Index		varlevelsup;	/* for subquery variables referencing outer
								 * relations; 0 in a normal var, >0 means N
								 * levels up */
	Index		varnosyn;		/* syntactic relation index (0 if unknown) */
	AttrNumber	varattnosyn;	/* syntactic attribute number */
	int			location;		/* token location, or -1 if unknown */
} Var;
```