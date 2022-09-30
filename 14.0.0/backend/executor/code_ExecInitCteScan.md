#1.ExecInitCteScan

```
ExecInitCteScan
--scanstate->ss.ps.ExecProcNode = ExecCteScan;
--prmdata = &(estate->es_param_exec_vals[node->cteParam]);
--scanstate->leader = castNode(CteScanState, DatumGetPointer(prmdata->value));
--if (scanstate->leader == NULL)
----prmdata->value = PointerGetDatum(scanstate);                        
----scanstate->leader = scanstate;                                      
----scanstate->cte_table = tuplestore_begin_heap(true, false, work_mem);
----tuplestore_set_eflags(scanstate->cte_table, scanstate->eflags);     
--else
----scanstate->readptr =
			tuplestore_alloc_read_pointer(scanstate->leader->cte_table,
										  scanstate->eflags);
----tuplestore_select_read_pointer(scanstate->leader->cte_table,
									   scanstate->readptr);
----tuplestore_rescan(scanstate->leader->cte_table);
```