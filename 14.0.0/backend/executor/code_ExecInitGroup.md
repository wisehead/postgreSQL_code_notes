#1.ExecInitGroup

```
ExecInitGroup
--grpstate->ss.ps.ExecProcNode = ExecGroup;
--outerPlanState(grpstate) = ExecInitNode(outerPlan(node), estate, eflags);
--tts_ops = ExecGetResultSlotOps(outerPlanState(&grpstate->ss), NULL);
--ExecCreateScanSlotFromOuterPlan(estate, &grpstate->ss, tts_ops);
--ExecInitResultTupleSlotTL(&grpstate->ss.ps, &TTSOpsVirtual);
--ExecAssignProjectionInfo(&grpstate->ss.ps, NULL);
--ExecInitQual(node->plan.qual, (PlanState *) grpstate);
--grpstate->eqfunction == execTuplesMatchPrepare
----ExecBuildGroupingEqual
```