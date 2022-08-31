#1.inheritance_planner

```cpp
/*
 * inheritance_planner
 *    Generate a plan in the case where the result relation is an
 *    inheritance set.
 *
 * We have to handle this case differently from cases where a source relation
 * is an inheritance set. Source inheritance is expanded at the bottom of the
 * plan tree (see allpaths.c), but target inheritance has to be expanded at
 * the top.  The reason is that for UPDATE, each target relation needs a
 * different targetlist matching its own column set.  Also, for both UPDATE
 * and DELETE, the executor needs the Append plan node at the top, else it
 * can't keep track of which table is the current target table.  Fortunately,
 * the UPDATE/DELETE target can never be the nullable side of an outer join,
 * so it's OK to generate the plan this way.
 *
 * Returns a query plan.
 */
static Plan *
inheritance_planner(PlannerInfo *root)

```

#2.caller
- subquery_planner