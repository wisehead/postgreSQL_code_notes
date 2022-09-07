#1.struct IvfflatScanOpaqueData

```cpp
typedef struct IvfflatScanOpaqueData
{
	int			probes;
	bool		first;
	Buffer		buf;

	/* Sorting */
	Tuplesortstate *sortstate;
	TupleDesc	tupdesc;
	TupleTableSlot *slot;
	bool		isnull;

	/* Support functions */
	FmgrInfo   *procinfo;
	FmgrInfo   *normprocinfo;
	Oid			collation;

	/* Lists */
	pairingheap *listQueue;
	IvfflatScanList lists[FLEXIBLE_ARRAY_MEMBER];	/* must come last */
}			IvfflatScanOpaqueData;

```