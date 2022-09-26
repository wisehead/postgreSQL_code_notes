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
                                                 

----
```