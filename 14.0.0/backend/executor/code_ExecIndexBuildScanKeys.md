#1.ExecIndexBuildScanKeys

```
ExecIndexBuildScanKeys
--foreach(qual_cell, quals)
----Expr	   *clause = (Expr *) lfirst(qual_cell);
----ScanKey		this_scan_key = &scan_keys[j++];
----if (IsA(clause, OpExpr))
------leftop = (Expr *) get_leftop(clause);
------varattno = ((Var *) leftop)->varattno;
------opfamily = index->rd_opfamily[varattno - 1];
------get_op_opfamily_properties(opno, opfamily, isorderby,
									   &op_strategy,
									   &op_lefttype,
									   &op_righttype);
------rightop = (Expr *) get_rightop(clause);
------runtime_keys[n_runtime_keys].scan_key = this_scan_key;
------runtime_keys[n_runtime_keys].key_expr =
					ExecInitExpr(rightop, planstate);
------runtime_keys[n_runtime_keys].key_toastable =
					TypeIsToastable(op_righttype);
------ScanKeyEntryInitialize(this_scan_key,
								   flags,
							varattno,	/* attribute number to scan */
							op_strategy, /* op's strategy */
							op_righttype,/* strategy subtype */
						 ((OpExpr *) clause)->inputcollid,/* collation */
								   opfuncid,	/* reg proc to use */
								   scanvalue);/* constant */
```