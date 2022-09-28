#1.ExecPushExprSlots

```
ExecPushExprSlots
--if (info->last_inner > 0)
----scratch.opcode = EEOP_INNER_FETCHSOME;       
----scratch.d.fetch.last_var = info->last_inner; 
----scratch.d.fetch.fixed = false;               
----scratch.d.fetch.kind = NULL;                 
----scratch.d.fetch.known_desc = NULL;           
----if (ExecComputeSlotInfo(state, &scratch))    
------ExprEvalPushStep(state, &scratch);         
--if (info->last_outer > 0)
	{
		scratch.opcode = EEOP_OUTER_FETCHSOME;
		scratch.d.fetch.last_var = info->last_outer;
		scratch.d.fetch.fixed = false;
		scratch.d.fetch.kind = NULL;
		scratch.d.fetch.known_desc = NULL;
		if (ExecComputeSlotInfo(state, &scratch))
			ExprEvalPushStep(state, &scratch);
	}
--if (info->last_scan > 0)
	{
		scratch.opcode = EEOP_SCAN_FETCHSOME;
		scratch.d.fetch.last_var = info->last_scan;
		scratch.d.fetch.fixed = false;
		scratch.d.fetch.kind = NULL;
		scratch.d.fetch.known_desc = NULL;
		if (ExecComputeSlotInfo(state, &scratch))
			ExprEvalPushStep(state, &scratch);
	}
```