#1.ExecInitBitmapHeapScan

```
ExecInitBitmapHeapScan
--scanstate->ss.ps.ExecProcNode = ExecBitmapHeapScan;//!!!!
--ExecOpenScanRelation
--outerPlanState(scanstate) = ExecInitNode(outerPlan(node), estate, eflags);
--ExecInitScanTupleSlot
--ExecInitResultTypeTL(&scanstate->ss.ps);
--ExecAssignScanProjectionInfo(&scanstate->ss);
--scanstate->ss.ps.qual =
		ExecInitQual(node->scan.plan.qual, (PlanState *) scanstate);
--scanstate->bitmapqualorig =
		ExecInitQual(node->bitmapqualorig, (PlanState *) scanstate);
--scanstate->prefetch_maximum = get_tablespace_io_concurrency
--table_beginscan_bm
----rel->rd_tableam->scan_begin(rel, snapshot, nkeys, key, NULL, flags);
------heap_beginscan
```