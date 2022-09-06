#1.subquery_planner

```cpp
/*--------------------
 * subquery_planner
 *    Invokes the planner on a subquery.  We recurse to here for each
 *    sub-SELECT found in the query tree.
 *
 * glob is the global state for the current planner run.
 * parse is the querytree produced by the parser & rewriter.
 * parent_root is the immediate parent Query's info (NULL at the top level).
 * hasRecursion is true if this is a recursive WITH query.
 * tuple_fraction is the fraction of tuples we expect will be retrieved.
 * tuple_fraction is interpreted as explained for grouping_planner, below.
 *
 * If subroot isn't NULL, we pass back the query's final PlannerInfo struct;
 * among other things this tells the output sort ordering of the plan.
 *
 * Basically, this routine does the stuff that should only be done once
 * per Query object.  It then calls grouping_planner.  At one time,
 * grouping_planner could be invoked recursively on the same Query object;
 * that's not currently true, but we keep the separation between the two
 * routines anyway, in case we need it again someday.
 *
 * subquery_planner will be called recursively to handle sub-Query nodes
 * found within the query's expressions and rangetable.
 *
 * Returns a query plan.
 *--------------------
 */
Plan *
subquery_planner(PlannerGlobal *glob, Query *parse,
                 PlannerInfo *parent_root,
                 bool hasRecursion, double tuple_fraction,
                 PlannerInfo **subroot)

subquery_planner
--pull_up_sublinks
--pull_up_subqueries
--preprocess_expression
--preprocess_qual_conditions
--inheritance_planner
--grouping_planner
--SS_finalize_plan
```

#2.caller

- set_subquery_pathlist
- standard_planner
- make_subplan
- SS_process_ctes
- recurse_set_operations