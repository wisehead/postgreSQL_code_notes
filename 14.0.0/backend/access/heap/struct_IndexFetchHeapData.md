#1.struct IndexFetchHeapData

```cpp
/*
 * Descriptor for fetches from heap via an index.
 */
typedef struct IndexFetchHeapData
{
	IndexFetchTableData xs_base;	/* AM independent part of the descriptor */

	Buffer		xs_cbuf;		/* current heap buffer in scan, if any */
	/* NB: if xs_cbuf is not InvalidBuffer, we hold a pin on that buffer */
} IndexFetchHeapData;
```