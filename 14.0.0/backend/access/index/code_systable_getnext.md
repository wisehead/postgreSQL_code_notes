#1.systable_getnext

```
systable_getnext
--if (sysscan->irel)
----index_getnext_slot
------tid = index_getnext_tid(scan, direction);
--------found = scan->indexRelation->rd_indam->amgettuple(scan, direction);
----------ivfflatgettuple
------index_fetch_heap(scan, slot))
--else //if (sysscan->irel)
----
```