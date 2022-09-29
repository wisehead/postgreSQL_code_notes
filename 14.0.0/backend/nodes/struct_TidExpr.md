#1.struct TidExpr

```cpp
/* one element in tss_tidexprs */
typedef struct TidExpr
{
	ExprState  *exprstate;		/* ExprState for a TID-yielding subexpr */
	bool		isarray;		/* if true, it yields tid[] not just tid */
	CurrentOfExpr *cexpr;		/* alternatively, we can have CURRENT OF */
} TidExpr;
```