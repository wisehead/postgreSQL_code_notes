#1.set_rel_pathlist


```cpp

/*
 * set_rel_pathlist
 *    Build access paths for a base relation
 */
static void
set_rel_pathlist(PlannerInfo *root, RelOptInfo *rel,
                 Index rti, RangeTblEntry *rte)
{
    if (rte->inh)
    {
        /* It's an "append relation", process accordingly */
        set_append_rel_pathlist(root, rel, rti, rte);
    }
    else if (rel->rtekind == RTE_SUBQUERY)
    {
        /* Subquery --- generate a separate plan for it */
        set_subquery_pathlist(root, rel, rti, rte);
    }
    else if (rel->rtekind == RTE_FUNCTION)
    {
        /* RangeFunction --- generate a suitable path for it */
        set_function_pathlist(root, rel, rte);
    }
    else if (rel->rtekind == RTE_VALUES)
    {
        /* Values list --- generate a suitable path for it */
        set_values_pathlist(root, rel, rte);
    }
    else if (rel->rtekind == RTE_CTE)
    {
        /* CTE reference --- generate a suitable path for it */
        if (rte->self_reference)
            set_worktable_pathlist(root, rel, rte);
        else
            set_cte_pathlist(root, rel, rte);
    }
    else
    {
        /* Plain relation */
        Assert(rel->rtekind == RTE_RELATION);
        set_plain_rel_pathlist(root, rel, rte);
    }

#ifdef OPTIMIZER_DEBUG
    debug_print_rel(root, rel);
#endif
}    
```