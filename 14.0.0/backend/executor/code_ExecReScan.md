#2.ExecReScan

```
ExecReScan
--if (node->chgParam != NULL)
----foreach(l, node->initPlan)
------SubPlanState *sstate = (SubPlanState *) lfirst(l);
------PlanState  *splan = sstate->planstate;
------if (splan->plan->extParam != NULL)
--------UpdateChangedParamSet(splan, node->chgParam);
------if (splan->chgParam != NULL)
--------ExecReScanSetParamPlan(sstate, node);
----foreach(l, node->subPlan)
------SubPlanState *sstate = (SubPlanState *) lfirst(l);
------PlanState  *splan = sstate->planstate;
------if (splan->plan->extParam != NULL)
--------UpdateChangedParamSet(splan, node->chgParam);
----if (node->lefttree != NULL)
------UpdateChangedParamSet(node->lefttree, node->chgParam);
----if (node->righttree != NULL)
------UpdateChangedParamSet(node->righttree, node->chgParam);
--if (node->ps_ExprContext)
----ReScanExprContext(node->ps_ExprContext);
--switch (nodeTag(node))
----case.....
```