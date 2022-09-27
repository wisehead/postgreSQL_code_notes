#1.struct TStoreState

```cpp
typedef struct
{
	DestReceiver pub;
	/* parameters: */
	Tuplestorestate *tstore;	/* where to put the data */
	MemoryContext cxt;			/* context containing tstore */
	bool		detoast;		/* were we told to detoast? */
	TupleDesc	target_tupdesc; /* target tupdesc, or NULL if none */
	const char *map_failure_msg;	/* tupdesc mapping failure message */
	/* workspace: */
	Datum	   *outvalues;		/* values array for result tuple */
	Datum	   *tofree;			/* temp values to be pfree'd */
	TupleConversionMap *tupmap; /* conversion map, if needed */
	TupleTableSlot *mapslot;	/* slot for mapped tuples */
} TStoreState;
```