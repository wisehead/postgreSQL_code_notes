#1.ExecPrepareExpr

```
ExecPrepareExpr
--node = expression_planner(node);
----result = eval_const_expressions(NULL, (Node *) expr);
------eval_const_expressions_mutator
----fix_opfuncids
--result = ExecInitExpr(node, NULL);
```