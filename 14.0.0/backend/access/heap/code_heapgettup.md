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
```