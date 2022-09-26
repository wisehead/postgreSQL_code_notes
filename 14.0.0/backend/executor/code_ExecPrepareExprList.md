#1.ExecPrepareExprList

```
ExecPrepareExprList
--foreach(lc, nodes)
----Expr	   *e = (Expr *) lfirst(lc);
----result = lappend(result, ExecPrepareExpr(e, estate));
```