#1.ExecResult

```
ExecResult
--if (node->rs_checkqual)
----ExecQual(node->resconstantqual, econtext);
------ExecEvalExprSwitchContext
--------state->evalfunc(state, econtext, isNull);
--if (!node->rs_done)
----outerPlan = outerPlanState(node);
----if (outerPlan != NULL)
------outerTupleSlot = ExecProcNode(outerPlan);
------econtext->ecxt_outertuple = outerTupleSlot;
----else
------node->rs_done = true;
----return ExecProject(node->ps.ps_ProjInfo);
--return NULL;
```