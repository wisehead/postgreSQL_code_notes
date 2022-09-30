#1.ExecCteScan

```
ExecCteScan
--ExecScan(&node->ss,
					(ExecScanAccessMtd) CteScanNext,
					(ExecScanRecheckMtd) CteScanRecheck);

```

#2. CteScanNext

```
CteScanNext
--tuplestorestate = node->leader->cte_table;
--tuplestore_select_read_pointer(tuplestorestate, node->readptr);
--slot = node->ss.ss_ScanTupleSlot;
--eof_tuplestore = tuplestore_ateof(tuplestorestate);
--tuplestore_gettupleslot
--if (eof_tuplestore && !node->leader->eof_cte)
----cteslot = ExecProcNode(node->cteplanstate);
----tuplestore_select_read_pointer(tuplestorestate, node->readptr);
----tuplestore_puttupleslot(tuplestorestate, cteslot);
----return ExecCopySlot(slot, cteslot);
```