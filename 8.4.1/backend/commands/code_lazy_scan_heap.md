#1.lazy_scan_heap

```cpp
/*
 *  lazy_scan_heap() -- scan an open heap relation
 *
 *      This routine sets commit status bits, builds lists of dead tuples
 *      and pages with free space, and calculates statistics on the number
 *      of live tuples in the heap.  When done, or when we run low on space
 *      for dead-tuple TIDs, invoke vacuuming of indexes and heap.
 *
 *      If there are no indexes then we just vacuum each dirty page as we
 *      process it, since there's no point in gathering many tuples.
 */
static void
lazy_scan_heap(Relation onerel, LVRelStats *vacrelstats,
               Relation *Irel, int nindexes, bool scan_all)
```