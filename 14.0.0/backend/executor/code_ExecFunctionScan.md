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
----if (rsinfo.returnMode == SFRM_ValuePerCall)
------if (first_time)
--------tupstore = tuplestore_begin_heap(randomAccess, false, work_mem);
--------tupdesc = CreateTemplateTupleDesc(1);
--------TupleDescInitEntry
------if (returnsTuple)
--------if (!fcinfo->isnull)
----------
```