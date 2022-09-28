#1. ExecInitSeqScan

```
ExecInitSeqScan
--scanstate = makeNode(SeqScanState);
--scanstate->ss.ps.plan = (Plan *) node;
--scanstate->ss.ps.state = estate;
--scanstate->ss.ps.ExecProcNode = ExecSeqScan;//!!!!!!
--scanstate->ss.ss_currentRelation =
		ExecOpenScanRelation(estate,
							 node->scanrelid,
							 eflags);
----ExecGetRangeTableRelation
--ExecInitScanTupleSlot(estate, &scanstate->ss,
						RelationGetDescr(scanstate->ss.ss_currentRelation),
						table_slot_callbacks(scanstate->ss.ss_currentRelation));
--ExecInitResultTypeTL(&scanstate->ss.ps);
--ExecAssignScanProjectionInfo(&scanstate->ss);
--scanstate->ss.ps.qual =
		ExecInitQual(node->plan.qual, (PlanState *) scanstate);
```