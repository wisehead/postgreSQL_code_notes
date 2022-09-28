#1.ExecInitSubPlan

```
ExecInitSubPlan
--SubPlanState *sstate = makeNode(SubPlanState);
--EState	   *estate = parent->state;
--sstate->subplan = subplan;
	/* Link the SubPlanState to already-initialized subplan */
--sstate->planstate = (PlanState *) list_nth(estate->es_subplanstates,
											   subplan->plan_id - 1);
--sstate->parent = parent;
--sstate->testexpr = ExecInitExpr((Expr *) subplan->testexpr, parent);
--sstate->args = ExecInitExprList(subplan->args, parent);
----foreach(lc, nodes)
------result = lappend(result, ExecInitExpr(e, parent));
--if (subplan->useHashTable)
----if (IsA(subplan->testexpr, OpExpr))
------oplist = list_make1(subplan->testexpr);
----else if (is_andclause(subplan->testexpr))
------oplist = castNode(BoolExpr, subplan->testexpr)->args;
```