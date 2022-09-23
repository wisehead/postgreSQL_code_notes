#1.index_endscan

```
index_endscan
--if (scan->xs_heapfetch)
----table_index_fetch_end(scan->xs_heapfetch);
------scan->rel->rd_tableam->index_fetch_end(scan);
--------heapam_index_fetch_end
----------heapam_index_fetch_reset
------------ReleaseBuffer(hscan->xs_cbuf);
----scan->xs_heapfetch = NULL;
--scan->indexRelation->rd_indam->amendscan(scan);
----ivfflatendscan
--RelationDecrementReferenceCount(scan->indexRelation);
--if (scan->xs_temp_snap)
----UnregisterSnapshot(scan->xs_snapshot);
------UnregisterSnapshotFromOwner
--IndexScanEnd(scan);
```