#1.pg_plan_queries

```cpp
/*
 * Generate plans for a list of already-rewritten queries.
 *
 * Normal optimizable statements generate PlannedStmt entries in the result
 * list.  Utility statements are simply represented by their statement nodes.
 */
List *
pg_plan_queries(List *querytrees, int cursorOptions, ParamListInfo boundParams)
```

#2.caller

- PrepareQuery
- _SPI_prepare_plan
- exec_simple_query
- exec_parse_message
- exec_bind_message
- RevalidateCachedPlan