#1.ExecSeqScan

```
ExecSeqScan
--ExecScan(&node->ss,
					(ExecScanAccessMtd) SeqNext,//!!!!!
					(ExecScanRecheckMtd) SeqRecheck);//!!!!!!
```

#2.ExecScan
```
ExecScan
--qual = node->ps.qual;
--projInfo = node->ps.ps_ProjInfo;
--econtext = node->ps.ps_ExprContext;
--if (!qual && !projInfo)
----return ExecScanFetch(node, accessMtd, recheckMtd);
--for (;;)
----slot = ExecScanFetch(node, accessMtd, recheckMtd);
----econtext->ecxt_scantuple = slot;
----if (qual == NULL || ExecQual(qual, econtext))
------if (projInfo)
--------return ExecProject(projInfo);
```