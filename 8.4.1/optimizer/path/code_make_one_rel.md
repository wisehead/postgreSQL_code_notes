#1.make_one_rel

```cpp

/*
 * make_one_rel                                                                                                                          *    Finds all possible access paths for executing a query, returning a
 *    single rel that represents the join of all base rels in the query.
 */
RelOptInfo *
make_one_rel(PlannerInfo *root, List *joinlist)
```

#2.caller

- query_planner