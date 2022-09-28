#1.struct TIDBitmap

```cpp
/*
 * Here is the representation for a whole TIDBitMap:
 */
struct TIDBitmap
{
	NodeTag		type;			/* to make it a valid Node */
	MemoryContext mcxt;			/* memory context containing me */
	TBMStatus	status;			/* see codes above */
	struct pagetable_hash *pagetable;	/* hash table of PagetableEntry's */
	int			nentries;		/* number of entries in pagetable */
	int			maxentries;		/* limit on same to meet maxbytes */
	int			npages;			/* number of exact entries in pagetable */
	int			nchunks;		/* number of lossy entries in pagetable */
	TBMIteratingState iterating;	/* tbm_begin_iterate called? */
	uint32		lossify_start;	/* offset to start lossifying hashtable at */
	PagetableEntry entry1;		/* used when status == TBM_ONE_PAGE */
	/* these are valid when iterating is true: */
	PagetableEntry **spages;	/* sorted exact-page list, or NULL */
	PagetableEntry **schunks;	/* sorted lossy-chunk list, or NULL */
	dsa_pointer dsapagetable;	/* dsa_pointer to the element array */
	dsa_pointer dsapagetableold;	/* dsa_pointer to the old element array */
	dsa_pointer ptpages;		/* dsa_pointer to the page array */
	dsa_pointer ptchunks;		/* dsa_pointer to the chunk array */
	dsa_area   *dsa;			/* reference to per-query dsa area */
};

```