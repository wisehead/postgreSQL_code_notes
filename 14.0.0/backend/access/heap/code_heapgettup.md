#1.heapgettup

```
heapgettup
--if (ScanDirectionIsForward(dir))
----if (!scan->rs_inited)
------if (scan->rs_base.rs_parallel != NULL)
--------ParallelBlockTableScanDesc pbscan =
				(ParallelBlockTableScanDesc) scan->rs_base.rs_parallel;
--------ParallelBlockTableScanWorker pbscanwork =
				scan->rs_parallelworkerdata;
--------table_block_parallelscan_startblock_init
--------page = table_block_parallelscan_nextpage(scan->rs_base.rs_rd, 				pbscanwork, pbscan);
------else
--------page = scan->rs_startblock
------heapgetpage((TableScanDesc) scan, page);
------lineoff = FirstOffsetNumber;	/* first offnum */
------scan->rs_inited = true;
```