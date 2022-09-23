#1.table_block_parallelscan_startblock_init

```
table_block_parallelscan_startblock_init
--pbscanwork->phsw_chunk_size = pg_nextpower2_32(Max(pbscan->phs_nblocks /
												PARALLEL_SEQSCAN_NCHUNKS, 1));
--pbscanwork->phsw_chunk_size = Min(pbscanwork->phsw_chunk_size,
									  PARALLEL_SEQSCAN_MAX_CHUNK_SIZE);
--retry:
--SpinLockAcquire(&pbscan->phs_mutex);
--sync_startpage = ss_get_location(rel, pbscan->phs_nblocks);
--SpinLockRelease(&pbscan->phs_mutex);
```

#2.ss_get_location

```
ss_get_location
--startloc = ss_search(rel->rd_node, 0, false);
```