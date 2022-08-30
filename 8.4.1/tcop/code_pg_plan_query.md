#1.pg_plan_query

```cpp
/*
 * Generate a plan for a single already-rewritten query.
 * This is a thin wrapper around planner() and takes the same parameters.
 */
PlannedStmt *
pg_plan_query(Query *querytree, int cursorOptions, ParamListInfo boundParams)


pg_plan_query
--planner

```

#2.caller
- pg_plan_queries
- init_execution_state
- ExplainOneQuery
