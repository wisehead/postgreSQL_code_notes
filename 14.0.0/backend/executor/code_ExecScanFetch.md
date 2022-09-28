#1.ExecScanFetch

```
ExecScanFetch
--Index		scanrelid = ((Scan *) node->ps.plan)->scanrelid;
--if (scanrelid == 0)
----TupleTableSlot *slot = node->ss_ScanTupleSlot;
----(!(*recheckMtd) (node, slot))
------SeqRecheck
----return slot;
--else if (epqstate->relsubs_done[scanrelid - 1])
----return ExecClearTuple(slot);
--else if (epqstate->relsubs_slot[scanrelid - 1] != NULL)
----//todo
--else if (epqstate->relsubs_rowmark[scanrelid - 1] != NULL)
----EvalPlanQualFetchRowMark
```