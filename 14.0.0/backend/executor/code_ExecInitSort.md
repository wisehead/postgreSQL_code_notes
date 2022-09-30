#1.ExecInitSort

```cpp
ExecInitSort
--sortstate->ss.ps.ExecProcNode = ExecSort;
--outerPlanState(sortstate) = ExecInitNode(outerPlan(node), estate, eflags);
--ExecCreateScanSlotFromOuterPlan(estate, &sortstate->ss, &TTSOpsVirtual);
--ExecInitResultTupleSlotTL
```