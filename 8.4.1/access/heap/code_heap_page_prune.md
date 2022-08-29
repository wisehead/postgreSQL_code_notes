#1.heap_page_prune

```cpp
/*
 * Prune and repair fragmentation in the specified page.
 *
 * Caller must have pin and buffer cleanup lock on the page.
 *
 * OldestXmin is the cutoff XID used to distinguish whether tuples are DEAD
 * or RECENTLY_DEAD (see HeapTupleSatisfiesVacuum).
 *
 * If redirect_move is set, we remove redirecting line pointers by
 * updating the root line pointer to point directly to the first non-dead
 * tuple in the chain.  NOTE: eliminating the redirect changes the first
 * tuple's effective CTID, and is therefore unsafe except within VACUUM FULL.
 * The only reason we support this capability at all is that by using it,
 * VACUUM FULL need not cope with LP_REDIRECT items at all; which seems a
 * good thing since VACUUM FULL is overly complicated already.
 *
 * If report_stats is true then we send the number of reclaimed heap-only
 * tuples to pgstats.  (This must be FALSE during vacuum, since vacuum will
 * send its own new total to pgstats, and we don't want this delta applied
 * on top of that.)
 *
 * Returns the number of tuples deleted from the page.
 */
int
heap_page_prune(Relation relation, Buffer buffer, TransactionId OldestXmin,
                bool redirect_move, bool report_stats)
```