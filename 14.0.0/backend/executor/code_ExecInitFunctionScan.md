#1.ExecInitFunctionScan

```
ExecInitFunctionScan
--scanstate->ss.ps.ExecProcNode = ExecFunctionScan;
--foreach(lc, node->functions)
----RangeTblFunction *rtfunc = (RangeTblFunction *) lfirst(lc);
----Node	   *funcexpr = rtfunc->funcexpr;
----FunctionScanPerFuncState *fs = &scanstate->funcstates[i];
----ExecInitTableFunctionResult
```

#2. ExecInitTableFunctionResult

```
ExecInitTableFunctionResult
--SetExprState *state = makeNode(SetExprState);
--if (IsA(expr, FuncExpr))
----FuncExpr   *func = (FuncExpr *) expr;
----state->args = ExecInitExprList(func->args, parent);
----init_sexpr(func->funcid, func->inputcollid, expr, state, parent,
				   econtext->ecxt_per_query_memory, func->funcretset, false);
```

#3.init_sexpr

```
init_sexpr
--pg_proc_aclcheck
--fmgr_info_cxt(foid, &(sexpr->func), sexprCxt);
--if (sexpr->func.fn_retset && needDescForSRF)
----functypclass = get_expr_result_type(sexpr->func.fn_expr,
											&funcrettype,
											&tupdesc);
```