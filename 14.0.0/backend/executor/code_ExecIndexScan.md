#1. ExecIndexScan
```
ExecIndexScan
--if (node->iss_NumRuntimeKeys != 0 && !node->iss_RuntimeKeysReady)
----ExecReScan((PlanState *) node);
--if (node->iss_NumOrderByKeys > 0)
----return ExecScan(&node->ss,
						(ExecScanAccessMtd) IndexNextWithReorder,
						(ExecScanRecheckMtd) IndexRecheck);
--else
----return ExecScan(&node->ss,
						(ExecScanAccessMtd) IndexNext,
						(ExecScanRecheckMtd) IndexRecheck);
```

