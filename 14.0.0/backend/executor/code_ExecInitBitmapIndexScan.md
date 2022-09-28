#1.ExecInitBitmapIndexScan

```
ExecInitBitmapIndexScan
--indexstate->ss.ps.ExecProcNode = ExecBitmapIndexScan;
--indexstate->biss_RelationDesc = index_open(node->indexid, lockmode);
--ExecIndexBuildScanKeys
--indexstate->biss_ScanDesc =
		index_beginscan_bitmap(indexstate->biss_RelationDesc,
							   estate->es_snapshot,
							   indexstate->biss_NumScanKeys);
--index_rescan(indexstate->biss_ScanDesc,
					 indexstate->biss_ScanKeys, indexstate->biss_NumScanKeys,
					 NULL, 0);


```