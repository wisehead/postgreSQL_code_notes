#1.index_beginscan

```cpp
index_beginscan
--index_beginscan_internal
----RelationIncrementReferenceCount(indexRelation);
----scan = indexRelation->rd_indam->ambeginscan(indexRelation, nkeys,norderbys);
------ivfflatbeginscan
--table_index_fetch_begin
----rel->rd_tableam->index_fetch_begin(rel);
------heapam_index_fetch_begin
```