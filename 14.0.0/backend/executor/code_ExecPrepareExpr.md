#1.ExecPrepareExpr

```
ExecPrepareExpr
--node = expression_planner(node);
----result = eval_const_expressions(NULL, (Node *) expr);
----fix_opfuncids
--result = ExecInitExpr(node, NULL);
```