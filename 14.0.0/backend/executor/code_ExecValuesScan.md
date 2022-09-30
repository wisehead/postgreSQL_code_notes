#1.ExecValuesScan

```
ExecValuesScan
--ExecScan(&node->ss,
					(ExecScanAccessMtd) ValuesNext,
					(ExecScanRecheckMtd) ValuesRecheck);
```

#2.ValuesNext

```
ValuesNext
--if (curr_idx >= 0 && curr_idx < node->array_len)
--
```