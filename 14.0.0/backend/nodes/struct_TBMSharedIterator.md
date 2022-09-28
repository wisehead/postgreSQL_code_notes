#1.struct TBMSharedIterator

```cpp
/*
 * same as TBMIterator, but it is used for joint iteration, therefore this
 * also holds a reference to the shared state.
 */
struct TBMSharedIterator
{
	TBMSharedIteratorState *state;	/* shared state */
	PTEntryArray *ptbase;		/* pagetable element array */
	PTIterationArray *ptpages;	/* sorted exact page index list */
	PTIterationArray *ptchunks; /* sorted lossy page index list */
	TBMIterateResult output;	/* MUST BE LAST (because variable-size) */
};
```