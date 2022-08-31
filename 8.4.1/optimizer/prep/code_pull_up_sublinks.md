#1.pull_up_sublinks

```cpp
/*
 * pull_up_sublinks
 *      Attempt to pull up ANY and EXISTS SubLinks to be treated as
 *      semijoins or anti-semijoins.
 *
 * A clause "foo op ANY (sub-SELECT)" can be processed by pulling the
 * sub-SELECT up to become a rangetable entry and treating the implied
 * comparisons as quals of a semijoin.  However, this optimization *only*
 * works at the top level of WHERE or a JOIN/ON clause, because we cannot
 * distinguish whether the ANY ought to return FALSE or NULL in cases
 * involving NULL inputs.  Also, in an outer join's ON clause we can only
 * do this if the sublink is degenerate (ie, references only the nullable
 * side of the join).  In that case it is legal to push the semijoin
 * down into the nullable side of the join.  If the sublink references any
 * nonnullable-side variables then it would have to be evaluated as part
 * of the outer join, which makes things way too complicated.
 *
 * Under similar conditions, EXISTS and NOT EXISTS clauses can be handled
 * by pulling up the sub-SELECT and creating a semijoin or anti-semijoin.
 *
 * This routine searches for such clauses and does the necessary parsetree
 * transformations if any are found.
 *
 * This routine has to run before preprocess_expression(), so the quals
 * clauses are not yet reduced to implicit-AND format.  That means we need
 * to recursively search through explicit AND clauses, which are
 * probably only binary ANDs.  We stop as soon as we hit a non-AND item.
 */
void
pull_up_sublinks(PlannerInfo *root)


```

#2.stack

```

pull_up_sublinks
--pull_up_sublinks_jointree_recurse
```