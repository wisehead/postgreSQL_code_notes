#1.make_agg

```cpp

Agg *
make_agg(PlannerInfo *root, List *tlist, List *qual,
         AggStrategy aggstrategy,
         int numGroupCols, AttrNumber *grpColIdx, Oid *grpOperators,
         long numGroups, int numAggs,
         Plan *lefttree)
```