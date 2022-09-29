#1.struct SubqueryScan

```cpp
/* ----------------
 *		subquery scan node
 *
 * SubqueryScan is for scanning the output of a sub-query in the range table.
 * We often need an extra plan node above the sub-query's plan to perform
 * expression evaluations (which we can't push into the sub-query without
 * risking changing its semantics).  Although we are not scanning a physical
 * relation, we make this a descendant of Scan anyway for code-sharing
 * purposes.
 *
 * Note: we store the sub-plan in the type-specific subplan field, not in
 * the generic lefttree field as you might expect.  This is because we do
 * not want plan-tree-traversal routines to recurse into the subplan without
 * knowing that they are changing Query contexts.
 * ----------------
 */
typedef struct SubqueryScan
{
	Scan		scan;
	Plan	   *subplan;
} SubqueryScan;

```