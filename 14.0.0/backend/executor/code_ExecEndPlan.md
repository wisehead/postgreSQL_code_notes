#1.ExecEndPlan

```
ExecEndPlan
--ExecEndNode(planstate);//call callback
--foreach(l, estate->es_subplanstates)
----PlanState  *subplanstate = (PlanState *) lfirst(l);
----ExecEndNode(subplanstate);
--ExecResetTupleTable
--ExecCloseResultRelations(estate);
--ExecCloseRangeTableRelations(estate);
```