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
----else
------page = scan->rs_cblock; /* current page */
------lineoff =	OffsetNumberNext(ItemPointerGetOffsetNumber(&(tuple->t_self)));
----LockBuffer(scan->rs_cbuf, BUFFER_LOCK_SHARE);
----dp = BufferGetPage(scan->rs_cbuf);                           
----TestForOldSnapshot(snapshot, scan->rs_base.rs_rd, dp);       
----lines = PageGetMaxOffsetNumber(dp);                          
----/* page and lineoff now reference the physically next tid */                                                         
----linesleft = lines - lineoff + 1;                             
--else if (backward)
----//todo
--else
----page = ItemPointerGetBlockNumber(&(tuple->t_self));
----if (page != scan->rs_cblock)
------heapgetpage((TableScanDesc) scan, page);
----dp = BufferGetPage(scan->rs_cbuf);                            
----TestForOldSnapshot(snapshot, scan->rs_base.rs_rd, dp);        
----lineoff = ItemPointerGetOffsetNumber(&(tuple->t_self));       
----lpp = PageGetItemId(dp, lineoff);                             
----tuple->t_data = (HeapTupleHeader) PageGetItem((Page) dp, lpp);
----tuple->t_len = ItemIdGetLength(lpp);                          
----return;       
--lpp = PageGetItemId(dp, lineoff);                                                
--for (;;)
----while (linesleft > 0)
------tuple->t_data = (HeapTupleHeader) PageGetItem((Page) dp, lpp); 
------tuple->t_len = ItemIdGetLength(lpp);                           
------ItemPointerSet(&(tuple->t_self), page, lineoff); 
------if (valid)              
--------LockBuffer(scan->rs_cbuf, BUFFER_LOCK_UNLOCK);
--------return;
--------linesleft;
----LockBuffer(scan->rs_cbuf, BUFFER_LOCK_UNLOCK);
----if (backward)
------//todo
----else if (scan->rs_base.rs_parallel != NULL)
------ParallelBlockTableScanDesc pbscan =                            
------(ParallelBlockTableScanDesc) scan->rs_base.rs_parallel;        
------ParallelBlockTableScanWorker pbscanwork =                      
------scan->rs_parallelworkerdata;                                                                                           
------page = table_block_parallelscan_nextpage(scan->rs_base.rs_rd,  
  										 pbscanwork, pbscan);                      
------finished = (page == InvalidBlockNumber);                       

----else
------page++;
------if (page >= scan->rs_nblocks)
--------page = 0;
------finished = (page == scan->rs_startblock) || (scan->rs_numblocks != InvalidBlockNumber ? --scan->rs_numblocks == 0 : false);
------if (scan->rs_base.rs_flags & SO_ALLOW_SYNC)
--------ss_report_location(scan->rs_base.rs_rd, page);
----if (finished)
		{
			if (BufferIsValid(scan->rs_cbuf))
				ReleaseBuffer(scan->rs_cbuf);
			scan->rs_cbuf = InvalidBuffer;
			scan->rs_cblock = InvalidBlockNumber;
			tuple->t_data = NULL;
			scan->rs_inited = false;
			return;
		}
----heapgetpage((TableScanDesc) scan, page);                
----LockBuffer(scan->rs_cbuf, BUFFER_LOCK_SHARE);           
----dp = BufferGetPage(scan->rs_cbuf);                      
----TestForOldSnapshot(snapshot, scan->rs_base.rs_rd, dp);  
----lines = PageGetMaxOffsetNumber((Page) dp);              
----linesleft = lines;                                      
----if (backward)
		{
			lineoff = lines;
			lpp = PageGetItemId(dp, lines);
		}
----else
		{
			lineoff = FirstOffsetNumber;
			lpp = PageGetItemId(dp, FirstOffsetNumber);
		}
```


















