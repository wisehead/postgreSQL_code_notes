#1.TidExprListCreate

```
TidExprListCreate
--foreach(l, node->tidquals)
----Expr	   *expr = (Expr *) lfirst(l);
----TidExpr    *tidexpr = (TidExpr *) palloc0(sizeof(TidExpr));
----


```