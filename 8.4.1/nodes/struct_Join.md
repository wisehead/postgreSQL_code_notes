#1.struct Join

```cpp
/* ----------------
 *      Join node
 *
 * jointype:    rule for joining tuples from left and right subtrees
 * joinqual:    qual conditions that came from JOIN/ON or JOIN/USING
 *              (plan.qual contains conditions that came from WHERE)
 *
 * When jointype is INNER, joinqual and plan.qual are semantically
 * interchangeable.  For OUTER jointypes, the two are *not* interchangeable;
 * only joinqual is used to determine whether a match has been found for
 * the purpose of deciding whether to generate null-extended tuples.
 * (But plan.qual is still applied before actually returning a tuple.)
 * For an outer join, only joinquals are allowed to be used as the merge
 * or hash condition of a merge or hash join.
 * ----------------
 */
typedef struct Join
{
    Plan        plan;
    JoinType    jointype;
    List       *joinqual;       /* JOIN quals (in addition to plan.qual) */
} Join;
```