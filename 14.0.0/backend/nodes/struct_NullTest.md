#1.struct NullTest

```cpp
typedef struct NullTest
{
	Expr		xpr;
	Expr	   *arg;			/* input expression */
	NullTestType nulltesttype;	/* IS NULL, IS NOT NULL */
	bool		argisrow;		/* T to perform field-by-field null checks */
	int			location;		/* token location, or -1 if unknown */
} NullTest;

```