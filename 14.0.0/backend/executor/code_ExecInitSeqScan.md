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
```