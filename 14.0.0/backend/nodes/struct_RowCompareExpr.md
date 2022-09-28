#1.struct RowCompareExpr

```cpp
typedef struct RowCompareExpr
{
	Expr		xpr;
	RowCompareType rctype;		/* LT LE GE or GT, never EQ or NE */
	List	   *opnos;			/* OID list of pairwise comparison ops */
	List	   *opfamilies;		/* OID list of containing operator families */
	List	   *inputcollids;	/* OID list of collations for comparisons */
	List	   *largs;			/* the left-hand input arguments */
	List	   *rargs;			/* the right-hand input arguments */
} RowCompareExpr;
```