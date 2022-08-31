#1.Agg

```cpp
typedef struct Agg
{
    Plan        plan;
    AggStrategy aggstrategy;
    int         numCols;        /* number of grouping columns */
    AttrNumber *grpColIdx;      /* their indexes in the target list */
    Oid        *grpOperators;   /* equality operators to compare with */
    long        numGroups;      /* estimated number of groups in input */
} Agg;
```