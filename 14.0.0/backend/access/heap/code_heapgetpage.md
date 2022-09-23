#1.heapgetpage

```
heapgetpage
--if (BufferIsValid(scan->rs_cbuf))
----ReleaseBuffer(scan->rs_cbuf);
----scan->rs_cbuf = InvalidBuffer;
--scan->rs_cbuf = ReadBufferExtended(scan->rs_base.rs_rd, MAIN_FORKNUM, 		page,RBM_NORMAL, scan->rs_strategy);
--scan->rs_cblock = page;
--buffer = scan->rs_cbuf;
--snapshot = scan->rs_base.rs_snapshot;
--heap_page_prune_opt
--LockBuffer(buffer, BUFFER_LOCK_SHARE);
--dp = BufferGetPage(buffer);
--TestForOldSnapshot
--lines = PageGetMaxOffsetNumber(dp);
--all_visible = PageIsAllVisible(dp) && !snapshot->takenDuringRecovery;
--for (lineoff = FirstOffsetNumber, lpp = PageGetItemId(dp, lineoff);
		 lineoff <= lines;
		 lineoff++, lpp++)
----if (ItemIdIsNormal(lpp))
------loctup.t_tableOid = RelationGetRelid(scan->rs_base.rs_rd);
------loctup.t_data = (HeapTupleHeader) PageGetItem((Page) dp, lpp);
------loctup.t_len = ItemIdGetLength(lpp);
------ItemPointerSet(&(loctup.t_self), page, lineoff);
------if (valid)
--------scan->rs_vistuples[ntup++] = lineoff;
--LockBuffer(buffer, BUFFER_LOCK_UNLOCK);
--scan->rs_ntuples = ntup;

```