#1. ExecInitExpr

```
ExecInitExpr
--state = makeNode(ExprState);
--state->expr = node;
--state->parent = parent;
--state->ext_params = NULL;
--ExecInitExprSlots(state, (Node *) node);
```