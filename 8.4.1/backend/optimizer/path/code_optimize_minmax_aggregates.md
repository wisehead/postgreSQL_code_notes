#1.optimize_minmax_aggregates

```cpp
/*
 * optimize_minmax_aggregates - check for optimizing MIN/MAX via indexes
 *
 * This checks to see if we can replace MIN/MAX aggregate functions by
 * subqueries of the form
 *      (SELECT col FROM tab WHERE ... ORDER BY col ASC/DESC LIMIT 1)
 * Given a suitable index on tab.col, this can be much faster than the
 * generic scan-all-the-rows plan.
 *
 * We are passed the preprocessed tlist, and the best path
 * devised for computing the input of a standard Agg node.  If we are able
 * to optimize all the aggregates, and the result is estimated to be cheaper
 * than the generic aggregate method, then generate and return a Plan that
 * does it that way.  Otherwise, return NULL.
 */
Plan *
optimize_minmax_aggregates(PlannerInfo *root, List *tlist, Path *best_path)
```