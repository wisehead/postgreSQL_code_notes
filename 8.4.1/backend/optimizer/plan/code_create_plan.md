#1.create_plan

```cpp
/*
 * create_plan
 *    Creates the access plan for a query by tracing backwards through the
 *    desired chain of pathnodes, starting at the node 'best_path'.  For
 *    every pathnode found, we create a corresponding plan node containing
 *    appropriate id, target list, and qualification information.
 *
 *    The tlists and quals in the plan tree are still in planner format,
 *    ie, Vars still correspond to the parser's numbering.  This will be
 *    fixed later by setrefs.c.
 *
 *    best_path is the best access path
 *
 *    Returns a Plan tree.
 */
Plan *
create_plan(PlannerInfo *root, Path *best_path)

```