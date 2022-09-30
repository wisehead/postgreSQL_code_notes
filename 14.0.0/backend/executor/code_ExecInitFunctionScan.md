#1.ExecInitFunctionScan

```
ExecInitFunctionScan
--scanstate->ss.ps.ExecProcNode = ExecFunctionScan;
--foreach(lc, node->functions)
----RangeTblFunction *rtfunc = (RangeTblFunction *) lfirst(lc);
----Node	   *funcexpr = rtfunc->funcexpr;
----FunctionScanPerFuncState *fs = &scanstate->funcstates[i];
```