#1.struct HeapTupleTableSlot

```cpp
typedef struct HeapTupleTableSlot
{
	TupleTableSlot base;

#define FIELDNO_HEAPTUPLETABLESLOT_TUPLE 1
	HeapTuple	tuple;			/* physical tuple */
#define FIELDNO_HEAPTUPLETABLESLOT_OFF 2
	uint32		off;			/* saved state for slot_deform_heap_tuple */
	HeapTupleData tupdata;		/* optional workspace for storing tuple */
} HeapTupleTableSlot;
```