#1.grouping_planner

```cpp
/*--------------------
 * grouping_planner
 *    Perform planning steps related to grouping, aggregation, etc.
 *    This primarily means adding top-level processing to the basic
 *    query plan produced by query_planner.
 *
 * tuple_fraction is the fraction of tuples we expect will be retrieved
 *
 * tuple_fraction is interpreted as follows:
 *    0: expect all tuples to be retrieved (normal case)
 *    0 < tuple_fraction < 1: expect the given fraction of tuples available
 *      from the plan to be retrieved
 *    tuple_fraction >= 1: tuple_fraction is the absolute number of tuples
 *      expected to be retrieved (ie, a LIMIT specification)
 *
 * Returns a query plan.  Also, root->query_pathkeys is returned as the
 * actual output ordering of the plan (in pathkey format).
 *--------------------
 */
static Plan *
grouping_planner(PlannerInfo *root, double tuple_fraction)
```

#2.caller

- subquery_planner
- inheritance_planner