#1.struct FunctionScanPerFuncState

```cpp
/*
 * Runtime data for each function being scanned.
 */
typedef struct FunctionScanPerFuncState
{
	SetExprState *setexpr;		/* state of the expression being evaluated */
	TupleDesc	tupdesc;		/* desc of the function result type */
	int			colcount;		/* expected number of result columns */
	Tuplestorestate *tstore;	/* holds the function result set */
	int64		rowcount;		/* # of rows in result set, -1 if not known */
	TupleTableSlot *func_slot;	/* function result slot (or NULL) */
} FunctionScanPerFuncState;

static TupleTableSlot *FunctionNext(FunctionScanState *node);
```