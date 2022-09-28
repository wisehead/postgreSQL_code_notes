#1.ExecScanFetch

```
ExecScanFetch
--Index		scanrelid = ((Scan *) node->ps.plan)->scanrelid;
--if (scanrelid == 0)
----TupleTableSlot *slot = node->ss_ScanTupleSlot;
----(!(*recheckMtd) (node, slot))
------SeqRecheck
----return slot;
```