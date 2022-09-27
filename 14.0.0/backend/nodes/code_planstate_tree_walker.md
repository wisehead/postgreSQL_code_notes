#1.planstate_tree_walker

```
planstate_tree_walker
--Plan	   *plan = planstate->plan;
--if (planstate_walk_subplans(planstate->initPlan, walker, context))
----return true;
--if (outerPlanState(planstate))//lefttree
----walker(outerPlanState(planstate), context))
--if (innerPlanState(planstate))//righttree
----(walker(innerPlanState(planstate), context))
--switch (nodeTag(plan))
----case T_Append:
------planstate_walk_members
------break;
----case T_MergeAppend:
------planstate_walk_members
------break;
----case T_BitmapAnd:
------planstate_walk_members
------break;
----case T_BitmapOr:
------planstate_walk_members
------break;
----case T_SubqueryScan:
------planstate_walk_members
------break;
----case T_CustomScan:
------planstate_walk_members
------break;
----default:
------break;
--(planstate_walk_subplans(planstate->subPlan, walker, context))
	
```

#2.planstate_walk_subplans

```
planstate_walk_subplans
--foreach(lc, plans)
----SubPlanState *sps = lfirst_node(SubPlanState, lc);
----(walker(sps->planstate, context))
```

#3.planstate_walk_members

```
planstate_walk_members
--for (j = 0; j < nplans; j++)
----(walker(planstates[j], context))
```