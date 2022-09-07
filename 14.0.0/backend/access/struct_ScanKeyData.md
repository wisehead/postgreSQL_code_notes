#1.struct ScanKeyData

```cpp
/*
 * A ScanKey represents the application of a comparison operator between
 * a table or index column and a constant.  When it's part of an array of
 * ScanKeys, the comparison conditions are implicitly ANDed.  The index
 * column is the left argument of the operator, if it's a binary operator.
 * (The data structure can support unary indexable operators too; in that
 * case sk_argument would go unused.  This is not currently implemented.)
 *
 * For an index scan, sk_strategy and sk_subtype must be set correctly for
 * the operator.  When using a ScanKey in a heap scan, these fields are not
 * used and may be set to InvalidStrategy/InvalidOid.
 *
 * If the operator is collation-sensitive, sk_collation must be set
 * correctly as well.
 *
 * A ScanKey can also represent a ScalarArrayOpExpr, that is a condition
 * "column op ANY(ARRAY[...])".  This is signaled by the SK_SEARCHARRAY
 * flag bit.  The sk_argument is not a value of the operator's right-hand
 * argument type, but rather an array of such values, and the per-element
 * comparisons are to be ORed together.
 *
 * A ScanKey can also represent a condition "column IS NULL" or "column
 * IS NOT NULL"; these cases are signaled by the SK_SEARCHNULL and
 * SK_SEARCHNOTNULL flag bits respectively.  The argument is always NULL,
 * and the sk_strategy, sk_subtype, sk_collation, and sk_func fields are
 * not used (unless set by the index AM).
 *
 * SK_SEARCHARRAY, SK_SEARCHNULL and SK_SEARCHNOTNULL are supported only
 * for index scans, not heap scans; and not all index AMs support them,
 * only those that set amsearcharray or amsearchnulls respectively.
 *
 * A ScanKey can also represent an ordering operator invocation, that is
 * an ordering requirement "ORDER BY indexedcol op constant".  This looks
 * the same as a comparison operator, except that the operator doesn't
 * (usually) yield boolean.  We mark such ScanKeys with SK_ORDER_BY.
 * SK_SEARCHARRAY, SK_SEARCHNULL, SK_SEARCHNOTNULL cannot be used here.
 *
 * Note: in some places, ScanKeys are used as a convenient representation
 * for the invocation of an access method support procedure.  In this case
 * sk_strategy/sk_subtype are not meaningful (but sk_collation can be); and
 * sk_func may refer to a function that returns something other than boolean.
 */
typedef struct ScanKeyData
{
	int			sk_flags;		/* flags, see below */
	AttrNumber	sk_attno;		/* table or index column number */
	StrategyNumber sk_strategy; /* operator strategy number */
	Oid			sk_subtype;		/* strategy subtype */
	Oid			sk_collation;	/* collation to use, if needed */
	FmgrInfo	sk_func;		/* lookup info for function to call */
	Datum		sk_argument;	/* data to compare */
} ScanKeyData;

```