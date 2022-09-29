#1.ExecInitTidScan

```
ExecInitTidScan
--tidstate->ss.ps.ExecProcNode = ExecTidScan;
--ExecOpenScanRelation
--ExecInitScanTupleSlot
--ExecInitResultTypeTL(&tidstate->ss.ps);
--ExecAssignScanProjectionInfo(&tidstate->ss);
--tidstate->ss.ps.qual =
		ExecInitQual(node->scan.plan.qual, (PlanState *) tidstate);
--TidExprListCreate(tidstate);
```