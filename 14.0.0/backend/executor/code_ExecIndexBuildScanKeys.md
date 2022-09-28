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
								   
----else if (IsA(clause, RowCompareExpr))
------forfour(largs_cell, rc->largs, rargs_cell, rc->rargs,
					opnos_cell, rc->opnos, collids_cell, rc->inputcollids)
--------ScanKey		this_sub_key = &first_sub_key[n_sub_key];
--------leftop = (Expr *) lfirst(largs_cell); 
--------rightop = (Expr *) lfirst(rargs_cell);
--------opno = lfirst_oid(opnos_cell);
--------varattno = ((Var *) leftop)->varattno;
--------opfamily = index->rd_opfamily[varattno - 1]
--------get_op_opfamily_properties
--------opfuncid = get_opfamily_proc   
--------runtime_keys[n_runtime_keys].scan_key = this_sub_key;    
--------runtime_keys[n_runtime_keys].key_expr =                  
		ExecInitExpr(rightop, planstate);                      
--------runtime_keys[n_runtime_keys].key_toastable =             
		TypeIsToastable(op_righttype);                         
--------n_runtime_keys++;                   
                     
----else if (IsA(clause, ScalarArrayOpExpr))
------ScalarArrayOpExpr *saop = (ScalarArrayOpExpr *) clause;
------leftop = (Expr *) linitial(saop->args);
------varattno = ((Var *) leftop)->varattno;
------opfamily = index->rd_opfamily[varattno - 1];
------get_op_opfamily_properties
------rightop = (Expr *) lsecond(saop->args);
------if (index->rd_indam->amsearcharray)
--------runtime_keys[n_runtime_keys].scan_key = this_scan_key;
--------runtime_keys[n_runtime_keys].key_expr =
						ExecInitExpr(rightop, planstate);
--------runtime_keys[n_runtime_keys].key_toastable = true;
--------n_runtime_keys++;
------else
--------array_keys[n_array_keys].scan_key = this_scan_key; 
--------array_keys[n_array_keys].array_expr =              
			ExecInitExpr(rightop, planstate);                
--------/* the remaining fields were zeroed by palloc0 */  
--------n_array_keys++;    
------ScanKeyEntryInitialize                                

					









```