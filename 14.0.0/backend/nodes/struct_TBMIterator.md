#1.struct TBMIterator

```cpp
/*
 * When iterating over a bitmap in sorted order, a TBMIterator is used to
 * track our progress.  There can be several iterators scanning the same
 * bitmap concurrently.  Note that the bitmap becomes read-only as soon as
 * any iterator is created.
 */
struct TBMIterator
{
	TIDBitmap  *tbm;			/* TIDBitmap we're iterating over */
	int			spageptr;		/* next spages index */
	int			schunkptr;		/* next schunks index */
	int			schunkbit;		/* next bit to check in current schunk */
	TBMIterateResult output;	/* MUST BE LAST (because variable-size) */
};
```