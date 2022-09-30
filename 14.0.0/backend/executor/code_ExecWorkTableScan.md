#1. ExecWorkTableScan

```
ExecWorkTableScan
--if (node->rustate == NULL)
----WorkTableScan *plan = (WorkTableScan *) node->ss.ps.plan;
----param = &(estate->es_param_exec_vals[plan->wtParam]);
----node->rustate = castNode(RecursiveUnionState, DatumGetPointer(param->value));
----ExecAssignScanType(&node->ss,
						   ExecGetResultType(&node->rustate->ps));
----ExecAssignScanProjectionInfo(&node->ss);
--ExecScan(&node->ss,
					(ExecScanAccessMtd) WorkTableScanNext,
					(ExecScanRecheckMtd) WorkTableScanRecheck);
```

#2. WorkTableScanNext

```
WorkTableScanNext
--tuplestorestate = node->rustate->working_table;
--slot = node->ss.ss_ScanTupleSlot;
--(void) tuplestore_gettupleslot(tuplestorestate, true, false, slot);
```

