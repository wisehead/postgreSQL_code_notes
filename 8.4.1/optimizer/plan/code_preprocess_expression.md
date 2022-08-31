#1.preprocess_expression

```cpp

/*
 * preprocess_expression
 *      Do subquery_planner's preprocessing work for an expression,
 *      which can be a targetlist, a WHERE clause (including JOIN/ON
 *      conditions), or a HAVING clause.
 */
static Node *
preprocess_expression(PlannerInfo *root, Node *expr, int kind)
```