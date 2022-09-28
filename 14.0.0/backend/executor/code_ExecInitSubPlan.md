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
----foreach(l, oplist)
------OpExpr	   *opexpr = lfirst_node(OpExpr, l);
      	    expr = (Expr *) linitial(opexpr->args);
			tle = makeTargetEntry(expr,
								  i,
								  NULL,
								  false);
			lefttlist = lappend(lefttlist, tle);

			/* Process righthand argument */
			expr = (Expr *) lsecond(opexpr->args);
			tle = makeTargetEntry(expr,
								  i,
								  NULL,
								  false);
			righttlist = lappend(righttlist, tle);
----//end foreach
----tupDescLeft = ExecTypeFromTL(lefttlist);
		slot = ExecInitExtraTupleSlot(estate, tupDescLeft, &TTSOpsVirtual);
		sstate->projLeft = ExecBuildProjectionInfo(lefttlist,
												   NULL,
												   slot,
												   parent,
												   NULL);

		sstate->descRight = tupDescRight = ExecTypeFromTL(righttlist);
		slot = ExecInitExtraTupleSlot(estate, tupDescRight, &TTSOpsVirtual);
		sstate->projRight = ExecBuildProjectionInfo
----sstate->cur_eq_comp = ExecBuildGroupingEqual
--//end if (subplan->useHashTable)
--return sstate;
```