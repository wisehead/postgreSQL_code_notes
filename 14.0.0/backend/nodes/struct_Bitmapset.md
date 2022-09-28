#1.struct Bitmapset

```cpp
typedef struct Bitmapset
{
	int			nwords;			/* number of words in array */
	bitmapword	words[FLEXIBLE_ARRAY_MEMBER];	/* really [nwords] */
} Bitmapset;
```