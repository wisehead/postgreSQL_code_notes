#1. BitmapHeapNext

```
BitmapHeapNext
--tbm = node->tbm;
--if (pstate == NULL)
----tbmiterator = node->tbmiterator;
--else
----shared_tbmiterator = node->shared_tbmiterator;
--tbmres = node->tbmres;
--if (!node->initialized)
----if (!pstate)
------tbm = (TIDBitmap *) MultiExecProcNode(outerPlanState(node));
------node->tbm = tbm;
------node->tbmiterator = tbmiterator = tbm_begin_iterate(tbm);
------node->tbmres = tbmres = NULL;
----else
------if (BitmapShouldInitializeSharedState(pstate))
--------tbm = (TIDBitmap *) MultiExecProcNode(outerPlanState(node));
--------node->tbm = tbm;
--------pstate->tbmiterator = tbm_prepare_shared_iterate(tbm);
--------BitmapDoneInitializingSharedState(pstate);
------node->shared_tbmiterator = shared_tbmiterator =
				tbm_attach_shared_iterate(dsa, pstate->tbmiterator);
--for (;;)
----if (tbmres == NULL)
------if (!pstate)
--------node->tbmres = tbmres = tbm_iterate(tbmiterator);
------else
--------node->tbmres = tbmres = tbm_shared_iterate(shared_tbmiterator);
------BitmapAdjustPrefetchIterator
------if (skip_fetch)
--------node->return_empty_tuples = tbmres->ntuples;
------else if (!table_scan_bitmap_next_block(scan, tbmres))
--------continue;
------BitmapAdjustPrefetchTarget(node);
----else
------BitmapPrefetch(node, scan);
------if (node->return_empty_tuples > 0)
--------ExecStoreAllNullTuple(slot);
------else
--------table_scan_bitmap_next_tuple(scan, tbmres, slot)
```