#1.ExecInitIndexScan

```
ExecInitIndexScan
--indexstate->ss.ps.ExecProcNode = ExecIndexScan//!!!!!
--ExecOpenScanRelation(estate, node->scan.scanrelid, eflags);
--ExecInitScanTupleSlot
--ExecInitResultTypeTL(&indexstate->ss.ps);
--ExecAssignScanProjectionInfo(&indexstate->ss);
--indexstate->ss.ps.qual =
		ExecInitQual(node->scan.plan.qual, (PlanState *) indexstate);
--indexstate->indexqualorig =
		ExecInitQual(node->indexqualorig, (PlanState *) indexstate);
--indexstate->indexorderbyorig =
		ExecInitExprList(node->indexorderbyorig, (PlanState *) indexstate);
--indexstate->iss_RelationDesc = index_open(node->indexid, lockmode);
--ExecIndexBuildScanKeys((PlanState *) indexstate,
						   indexstate->iss_RelationDesc,
						   node->indexqual,
						   false,
						   &indexstate->iss_ScanKeys,
						   &indexstate->iss_NumScanKeys,
						   &indexstate->iss_RuntimeKeys,
						   &indexstate->iss_NumRuntimeKeys,
						   NULL,	/* no ArrayKeys */
						   NULL);
--ExecIndexBuildScanKeys((PlanState *) indexstate,
						   indexstate->iss_RelationDesc,
						   node->indexorderby,
						   true,
						   &indexstate->iss_OrderByKeys,
						   &indexstate->iss_NumOrderByKeys,
						   &indexstate->iss_RuntimeKeys,
						   &indexstate->iss_NumRuntimeKeys,
						   NULL,	/* no ArrayKeys */
						   NULL);
```