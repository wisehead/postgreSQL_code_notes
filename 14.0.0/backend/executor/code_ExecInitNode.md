#1.ExecInitNode

```
ExecInitNode
--switch (nodeTag(node))
----case........
--ExecSetExecProcNode(result, result->ExecProcNode);
----node->ExecProcNodeReal = function;
----node->ExecProcNode = ExecProcNodeFirst;
--foreach(l, node->initPlan)
	{
		SubPlan    *subplan = (SubPlan *) lfirst(l);
		SubPlanState *sstate;

		Assert(IsA(subplan, SubPlan));
		sstate = ExecInitSubPlan(subplan, result);
		subps = lappend(subps, sstate);
	}
--result->initPlan = subps;
```