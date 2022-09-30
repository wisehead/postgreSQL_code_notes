#1.ExecFunctionScan

```
ExecFunctionScan
--ExecScan(&node->ss,
					(ExecScanAccessMtd) FunctionNext,
					(ExecScanRecheckMtd) FunctionRecheck);
```

#2. FunctionNext

```
FunctionNext
--if (node->simple)
----Tuplestorestate *tstore = node->funcstates[0].tstore;
----if (tstore == NULL)
------ExecMakeTableFunctionResult
----tuplestore_gettupleslot
```

#3. ExecMakeTableFunctionResult

```
ExecMakeTableFunctionResult
--for (;;)
----if (!setexpr->elidedFuncState)
------InitFunctionCallInfoData(*fcinfo, &(setexpr->func),
								 list_length(setexpr->args),
								 setexpr->fcinfo->fncollation,
								 NULL, (Node *) &rsinfo);
------ExecEvalFuncArgs(fcinfo, setexpr->args, econtext);
------result = FunctionCallInvoke(fcinfo);
----else
------InitFunctionCallInfoData(*fcinfo, NULL, 0, InvalidOid, NULL, NULL);
----if (rsinfo.returnMode == SFRM_ValuePerCall)
------if (first_time)
--------tupstore = tuplestore_begin_heap(randomAccess, false, work_mem);
--------tupdesc = CreateTemplateTupleDesc(1);
--------TupleDescInitEntry
------if (returnsTuple)
--------if (!fcinfo->isnull)
----------HeapTupleHeader td = DatumGetHeapTupleHeader(result);
----------if (tupdesc == NULL)
------------tupdesc = lookup_rowtype_tupdesc_copy
----------tmptup.t_len = HeapTupleHeaderGetDatumLength(td);
----------tmptup.t_data = td;
----------tuplestore_puttuple(tupstore, &tmptup);
--------else
----------tuplestore_putvalues(tupstore, expectedDesc, NULL, nullflags);
------else
--------tuplestore_putvalues(tupstore, tupdesc, &result, &fcinfo->isnull);
--if (rsinfo.setResult == NULL)
----tupstore = tuplestore_begin_heap(randomAccess, false, work_mem);
----rsinfo.setResult = tupstore;
----if (!returnsSet)
------tuplestore_putvalues(tupstore, expectedDesc, NULL, nullflags);
--if (rsinfo.setDesc)
----tupledesc_match(expectedDesc, rsinfo.setDesc);
```