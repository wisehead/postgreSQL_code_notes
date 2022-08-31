#1.query_planner

```cpp
/*
 * query_planner
 *    Generate a path (that is, a simplified plan) for a basic query,
 *    which may involve joins but not any fancier features.
 *
 * Since query_planner does not handle the toplevel processing (grouping,
 * sorting, etc) it cannot select the best path by itself.  It selects
 * two paths: the cheapest path that produces all the required tuples,
 * independent of any ordering considerations, and the cheapest path that
 * produces the expected fraction of the required tuples in the required
 * ordering, if there is a path that is cheaper for this than just sorting
 * the output of the cheapest overall path.  The caller (grouping_planner)
 * will make the final decision about which to use.
 *
 * Input parameters:
 * root describes the query to plan
 * tlist is the target list the query should produce
 *      (this is NOT necessarily root->parse->targetList!)
 * tuple_fraction is the fraction of tuples we expect will be retrieved
 * limit_tuples is a hard limit on number of tuples to retrieve,
 *      or -1 if no limit
 *
 * Output parameters:                                                                                                                    * *cheapest_path receives the overall-cheapest path for the query
 * *sorted_path receives the cheapest presorted path for the query,
 *              if any (NULL if there is no useful presorted path)
 * *num_groups receives the estimated number of groups, or 1 if query
 *              does not use grouping
 *
 * Note: the PlannerInfo node also includes a query_pathkeys field, which is
 * both an input and an output of query_planner().  The input value signals
 * query_planner that the indicated sort order is wanted in the final output
 * plan.  But this value has not yet been "canonicalized", since the needed
 * info does not get computed until we scan the qual clauses.  We canonicalize
 * it as soon as that task is done.  (The main reason query_pathkeys is a
 * PlannerInfo field and not a passed parameter is that the low-level routines
 * in indxpath.c need to see it.)
 *
 * Note: the PlannerInfo node also includes group_pathkeys, window_pathkeys,
 * distinct_pathkeys, and sort_pathkeys, which like query_pathkeys need to be
 * canonicalized once the info is available.
 *
 * tuple_fraction is interpreted as follows:
 *    0: expect all tuples to be retrieved (normal case)
 *    0 < tuple_fraction < 1: expect the given fraction of tuples available
 *      from the plan to be retrieved
 *    tuple_fraction >= 1: tuple_fraction is the absolute number of tuples
 *      expected to be retrieved (ie, a LIMIT specification)
 * Note that a nonzero tuple_fraction could come from outer context; it is
 * therefore not redundant with limit_tuples.  We use limit_tuples to determine
 * whether a bounded sort can be used at runtime.
 */
 
void
query_planner(PlannerInfo *root, List *tlist,
              double tuple_fraction, double limit_tuples,
              Path **cheapest_path, Path **sorted_path,
              double *num_groups)
```

#2.caller

- grouping_planner