#1.ExecInitMaterial

```
ExecInitMaterial
--matstate->ss.ps.ExecProcNode = ExecMaterial;
--outerPlan = outerPlan(node);
--outerPlanState(matstate) = ExecInitNode(outerPlan, estate, eflags);
--ExecInitResultTupleSlotTL
--ExecCreateScanSlotFromOuterPlan
```