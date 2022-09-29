#1. MultiExecBitmapIndexScan

```
MultiExecBitmapIndexScan
--if (node->biss_result)
----tbm = node->biss_result;
----node->biss_result = NULL;	/* reset for next time */
--else
----tbm = tbm_create(work_mem * 1024L,
						 ((BitmapIndexScan *) node->ss.ps.plan)->isshared ?
						 node->ss.ps.state->es_query_dsa : NULL);
--while (doscan)
----nTuples += (double) index_getbitmap(scandesc, tbm);
------ntids = scan->indexRelation->rd_indam->amgetbitmap(scan, bitmap);
--------btgetbitmap			
--doscan = ExecIndexAdvanceArrayKeys(node->biss_ArrayKeys,
										   node->biss_NumArrayKeys);
--if (doscan)			 
----index_rescan(node->biss_ScanDesc,
						 node->biss_ScanKeys, node->biss_NumScanKeys,
						 NULL, 0);
					
```

#2. index_rescan

```
index_rescan
--table_index_fetch_reset
----scan->rel->rd_tableam->index_fetch_reset(scan);
------heapam_index_fetch_reset
--------ReleaseBuffer(hscan->xs_cbuf);
--scan->indexRelation->rd_indam->amrescan(scan, keys, nkeys,
											orderbys, norderbys);
----btrescan											
```