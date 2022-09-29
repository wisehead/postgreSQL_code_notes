#1.ExecSubqueryScan

```
ExecSubqueryScan
--ExecScan(&node->ss,
					(ExecScanAccessMtd) SubqueryNext,
					(ExecScanRecheckMtd) SubqueryRecheck);
```

#2.SubqueryNext

```
SubqueryNext
--slot = ExecProcNode(node->subplan);
```