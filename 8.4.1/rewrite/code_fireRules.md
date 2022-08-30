#1.fireRules

```cpp
/*
 *  fireRules -
 *     Iterate through rule locks applying rules.
 *
 * Input arguments:
 *  parsetree - original query
 *  rt_index - RT index of result relation in original query
 *  event - type of rule event
 *  locks - list of rules to fire
 * Output arguments:
 *  *instead_flag - set TRUE if any unqualified INSTEAD rule is found
 *                  (must be initialized to FALSE)
 *  *returning_flag - set TRUE if we rewrite RETURNING clause in any rule
 *                  (must be initialized to FALSE)
 *  *qual_product - filled with modified original query if any qualified
 *                  INSTEAD rule is found (must be initialized to NULL)
 * Return value:
 *  list of rule actions adjusted for use with this query
 *
 * Qualified INSTEAD rules generate their action with the qualification
 * condition added.  They also generate a modified version of the original
 * query with the negated qualification added, so that it will run only for
 * rows that the qualified action doesn't act on.  (If there are multiple
 * qualified INSTEAD rules, we AND all the negated quals onto a single
 * modified original query.)  We won't execute the original, unmodified
 * query if we find either qualified or unqualified INSTEAD rules.  If
 * we find both, the modified original query is discarded too.
 */
static List *
fireRules(Query *parsetree,
          int rt_index,
          CmdType event,
          List *locks,
          bool *instead_flag,
          bool *returning_flag,
          Query **qual_product)

```

#2.caller

- RewriteQuery