#1. ExecInitExpr

```
ExecInitExpr
--state = makeNode(ExprState);
--state->expr = node;
--state->parent = parent;
--state->ext_params = NULL;
--ExecInitExprSlots(state, (Node *) node);
----get_last_attnums_walker
----ExecPushExprSlots
--ExecInitExprRec(node, state, &state->resvalue, &state->resnull);
--scratch.opcode = EEOP_DONE;
--ExprEvalPushStep(state, &scratch);
--ExecReadyExpr(state);
----jit_compile_expr
----ExecReadyInterpretedExpr
------ExecInitInterpreter
------state->evalfunc = ExecInterpExprStillValid;
```

#2.get_last_attnums_walker

```
get_last_attnums_walker
--if (IsA(node, Var))
----switch (variable->varno)
		{
			case INNER_VAR:
				info->last_inner = Max(info->last_inner, attnum);
				break;

			case OUTER_VAR:
				info->last_outer = Max(info->last_outer, attnum);
				break;

				/* INDEX_VAR is handled by default case */

			default:
				info->last_scan = Max(info->last_scan, attnum);
				break;
		}
----return false;
--return expression_tree_walker(node, get_last_attnums_walker,
								  (void *) info);
```