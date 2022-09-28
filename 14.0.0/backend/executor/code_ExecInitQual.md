#1.ExecInitQual

```
ExecInitQual
--state = makeNode(ExprState);
--state->expr = (Expr *) qual;
--state->parent = parent;
--state->flags = EEO_FLAG_IS_QUAL;
--ExecInitExprSlots(state, (Node *) qual);
----get_last_attnums_walker
--scratch.opcode = EEOP_QUAL;
--scratch.resvalue = &state->resvalue;
--scratch.resnull = &state->resnull;
--foreach(lc, qual)
----Expr	   *node = (Expr *) lfirst(lc);
----ExecInitExprRec(node, state, &state->resvalue, &state->resnull);
----scratch.d.qualexpr.jumpdone = -1;
----ExprEvalPushStep(state, &scratch);
----adjust_jumps = lappend_int(adjust_jumps,
								   state->steps_len - 1);
----foreach(lc, adjust_jumps)
------ExprEvalStep *as = &state->steps[lfirst_int(lc)];
------as->d.qualexpr.jumpdone = state->steps_len;
--scratch.opcode = EEOP_DONE;
--ExprEvalPushStep(state, &scratch);
--ExecReadyExpr(state);
```