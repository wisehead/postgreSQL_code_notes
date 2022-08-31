#1.make_sort

```cpp
/*
 * make_sort --- basic routine to build a Sort plan node
 *
 * Caller must have built the sortColIdx, sortOperators, and nullsFirst
 * arrays already.  limit_tuples is as for cost_sort (in particular, pass
 * -1 if no limit)
 */
static Sort *
make_sort(PlannerInfo *root, Plan *lefttree, int numCols,
          AttrNumber *sortColIdx, Oid *sortOperators, bool *nullsFirst,
          double limit_tuples)
```