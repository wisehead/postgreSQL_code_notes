#1.TidListEval

```
TidListEval
--table_beginscan_tid
----rel->rd_tableam->scan_begin(rel, snapshot, 0, NULL, NULL, flags);
------heap_beginscan
--foreach(l, tidstate->tss_tidexprs)
----TidExpr    *tidexpr = (TidExpr *) lfirst(l);
----if (tidexpr->exprstate && !tidexpr->isarray)
------ExecEvalExprSwitchContext
--------retDatum = state->evalfunc(state, econtext, isNull);
------table_tuple_tid_valid
------tidList[numTids++] = *itemptr;
----else if (tidexpr->exprstate && tidexpr->isarray)
------ExecEvalExprSwitchContext
------itemarray = DatumGetArrayTypeP(arraydatum);
------deconstruct_array(itemarray,
					TIDOID, sizeof(ItemPointerData), false,TYPALIGN_SHORT,
					&ipdatums, &ipnulls, &ndatums);
------for (i = 0; i < ndatums; i++)
------itemptr = (ItemPointer) DatumGetPointer(ipdatums[i]);
------tidList[numTids++] = *itemptr;
----else
------execCurrentOf
--if (numTids > 1)
----qsort((void *) tidList, numTids, sizeof(ItemPointerData),
			  itemptr_comparator);
----numTids = qunique(tidList, numTids, sizeof(ItemPointerData),
						  itemptr_comparator);
```