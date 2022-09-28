#1.ExecInitResult

```
ExecInitResult
--resstate = makeNode(ResultState);        
--resstate->ps.plan = (Plan *) node;       
--resstate->ps.state = estate;             
--resstate->ps.ExecProcNode = ExecResult;  
--resstate->rs_done = false;
--resstate->rs_checkqual = (node->resconstantqual == NULL) ? false : true;
--outerPlanState(resstate) = ExecInitNode(outerPlan(node), estate, eflags);
--ExecInitResultTupleSlotTL(&resstate->ps, &TTSOpsVirtual);
----ExecInitResultTypeTL
------tupDesc = ExecTypeFromTL(planstate->plan->targetlist);
------planstate->ps_ResultTupleDesc = tupDesc;
----ExecInitResultSlot
--ExecAssignProjectionInfo(&resstate->ps, NULL);
--resstate->ps.qual =
		ExecInitQual(node->plan.qual, (PlanState *) resstate);
--resstate->resconstantqual =
		ExecInitQual((List *) node->resconstantqual, (PlanState *) resstate);
```