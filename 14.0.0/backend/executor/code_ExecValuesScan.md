#1.ExecValuesScan

```
ExecValuesScan
--ExecScan(&node->ss,
					(ExecScanAccessMtd) ValuesNext,
					(ExecScanRecheckMtd) ValuesRecheck);
```

#2.ValuesNext

```
ValuesNext
--if (curr_idx >= 0 && curr_idx < node->array_len)
----List	   *exprlist = node->exprlists[curr_idx];
----List	   *exprstatelist = node->exprstatelists[curr_idx];
----if (exprstatelist == NIL)
------exprstatelist = ExecInitExprList(exprlist, NULL);
----foreach(lc, exprstatelist)
------ExprState  *estate = (ExprState *) lfirst(lc);
------Form_pg_attribute attr = TupleDescAttr(slot->tts_tupleDescriptor,
												   resind);
------values[resind] = ExecEvalExpr(estate,
										  econtext,
										  &isnull[resind]);
------values[resind] = MakeExpandedObjectReadOnly
----ExecStoreVirtualTuple(slot);			
```