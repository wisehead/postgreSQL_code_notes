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
--expression_tree_walker
```