#1.TidNext

```
TidNext
--if (node->tss_TidList == NULL)
----TidListEval(node);
--while (node->tss_TidPtr >= 0 && node->tss_TidPtr < numTids)
----ItemPointerData tid = tidList[node->tss_TidPtr];
----if (node->tss_isCurrentOf)
------table_tuple_get_latest_tid(scan, &tid);
----table_tuple_fetch_row_version
```