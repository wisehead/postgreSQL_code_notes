#1.table_block_parallelscan_nextpage

```cpp
table_block_parallelscan_nextpage
--if (pbscanwork->phsw_chunk_remaining > 0)
----nallocated = ++pbscanwork->phsw_nallocated;
----pbscanwork->phsw_chunk_remaining--;
--else
----nallocated = pbscanwork->phsw_nallocated =
			pg_atomic_fetch_add_u64(&pbscan->phs_nallocated,
									pbscanwork->phsw_chunk_size);
----pbscanwork->phsw_chunk_remaining = pbscanwork->phsw_chunk_size - 1;
--if (nallocated >= pbscan->phs_nblocks)
----page = InvalidBlockNumber;	/* all blocks have been allocated */
--else
----page = (nallocated + pbscan->phs_startblock) % pbscan->phs_nblocks;
--if (pbscan->base.phs_syncscan)
----if (page != InvalidBlockNumber)
------ss_report_location(rel, page);
----else if (nallocated == pbscan->phs_nblocks)
------ss_report_location(rel, pbscan->phs_startblock);
--return page;
```