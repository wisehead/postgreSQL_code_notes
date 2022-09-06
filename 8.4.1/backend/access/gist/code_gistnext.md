#1.gistnext

```cpp


/*
 * Fetch tuple(s) that match the search key; this can be invoked
 * either to fetch the first such tuple or subsequent matching tuples.
 *
 * This function is used by both gistgettuple and gistgetbitmap. When
 * invoked from gistgettuple, tbm is null and the next matching tuple
 * is returned in scan->xs_ctup.t_self.  When invoked from getbitmap,
 * tbm is non-null and all matching tuples are added to tbm before
 * returning.  In both cases, the function result is the number of
 * returned tuples.
 *
 * If scan specifies to skip killed tuples, continue looping until we find a
 * non-killed tuple that matches the search key.
 */
static int64
gistnext(IndexScanDesc scan, TIDBitmap *tbm)
```

#2.caller

- gistgettuple
- gistgettuple