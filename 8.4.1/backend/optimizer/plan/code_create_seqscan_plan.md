#1.create_seqscan_plan

```cpp

/*****************************************************************************
 *
 *  BASE-RELATION SCAN METHODS
 *
 *****************************************************************************/


/*
 * create_seqscan_plan
 *   Returns a seqscan plan for the base relation scanned by 'best_path'
 *   with restriction clauses 'scan_clauses' and targetlist 'tlist'.
 */
static SeqScan *
create_seqscan_plan(PlannerInfo *root, Path *best_path,
                    List *tlist, List *scan_clauses)
```