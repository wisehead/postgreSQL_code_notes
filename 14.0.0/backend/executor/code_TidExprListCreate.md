#1.TidExprListCreate

```
TidExprListCreate
--foreach(l, node->tidquals)
----Expr	   *expr = (Expr *) lfirst(l);
----TidExpr    *tidexpr = (TidExpr *) palloc0(sizeof(TidExpr));
----if (is_opclause(expr))
------arg1 = get_leftop(expr);
------arg2 = get_rightop(expr);
------if (IsCTIDVar(arg1))
--------tidexpr->exprstate = ExecInitExpr((Expr *) arg2,
												  &tidstate->ss.ps);
------else if (IsCTIDVar(arg2))
--------tidexpr->exprstate = ExecInitExpr((Expr *) arg1,
												  &tidstate->ss.ps);
------tidexpr->isarray = false;
----else if (expr && IsA(expr, ScalarArrayOpExpr))
------ScalarArrayOpExpr *saex = (ScalarArrayOpExpr *) expr;
------tidexpr->exprstate = ExecInitExpr(lsecond(saex->args),
											  &tidstate->ss.ps);
------tidexpr->isarray = true;
----else if (expr && IsA(expr, CurrentOfExpr))
------CurrentOfExpr *cexpr = (CurrentOfExpr *) expr;
------tidexpr->cexpr = cexpr;
------tidstate->tss_isCurrentOf = true;
----else
------elog(ERROR, "could not identify CTID expression");
----tidstate->tss_tidexprs = lappend(tidstate->tss_tidexprs, tidexpr);
```