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

#3.stack

```
make_one_rel
//生成基本关系的访问路径，即为每个基本关系生成一个RelOptInfo结构并生成路径，放在其RelOptInfo
//的pathlist字段中，对应动态规划算法的第1层
--set_base_rel_pathlists
//生成最终路径，返回一个代表链接所有基本关系的RelOptInfo结构，它的pathlist字段就是所有的最终路径组成的链表。对应第2层到第n层。
--make_rel_from_joinlist
```