#1.table_scan_bitmap_next_tuple

```
table_scan_bitmap_next_tuple
--scan->rs_rd->rd_tableam->scan_bitmap_next_block(scan, tbmres);
```


#2.heapam_scan_bitmap_next_tuple

```
heapam_scan_bitmap_next_tuple
--targoffset = hscan->rs_vistuples[hscan->rs_cindex];
--dp = (Page) BufferGetPage(hscan->rs_cbuf);
--lp = PageGetItemId(dp, targoffset);
--hscan->rs_ctup.t_data = (HeapTupleHeader) PageGetItem((Page) dp, lp);
--hscan->rs_ctup.t_len = ItemIdGetLength(lp);
--hscan->rs_ctup.t_tableOid = scan->rs_rd->rd_id;
--ExecStoreBufferHeapTuple(&hscan->rs_ctup,
							 slot,
							 hscan->rs_cbuf);
----tts_buffer_heap_store_tuple							 
```