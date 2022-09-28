#1.ExecComputeSlotInfo

```
ExecComputeSlotInfo
--PlanState  *parent = state->parent;
--if (op->d.fetch.known_desc != NULL)
	{
		desc = op->d.fetch.known_desc;
		tts_ops = op->d.fetch.kind;
		isfixed = op->d.fetch.kind != NULL;
	}
--else if (!parent)
----isfixed = false;
--else if (opcode == EEOP_INNER_FETCHSOME)
----PlanState  *is = innerPlanState(parent);//right tree
----if (parent->inneropsset && !parent->inneropsfixed)
------isfixed = false;
----else if (parent->inneropsset && parent->innerops)
------isfixed = true;                
------tts_ops = parent->innerops;    
------desc = ExecGetResultType(is);  
--------planstate->ps_ResultTupleDesc;
----else if (is)
------tts_ops = ExecGetResultSlotOps(is, &isfixed);
------desc = ExecGetResultType(is);
--else if (opcode == EEOP_OUTER_FETCHSOME)
----//todo ...
--else if (opcode == EEOP_SCAN_FETCHSOME)
----//todo ...

```