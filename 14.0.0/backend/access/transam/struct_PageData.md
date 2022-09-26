#1.struct PageData

```cpp
/* Struct of generic xlog data for single page */
typedef struct
{
	Buffer		buffer;			/* registered buffer */
	int			flags;			/* flags for this buffer */
	int			deltaLen;		/* space consumed in delta field */
	char	   *image;			/* copy of page image for modification, do not
								 * do it in-place to have aligned memory chunk */
	char		delta[MAX_DELTA_SIZE];	/* delta between page images */
} PageData;
```