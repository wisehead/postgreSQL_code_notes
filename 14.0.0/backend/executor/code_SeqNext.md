#1. SeqNext

```
SeqNext
--if (scandesc == NULL)
----scandesc = table_beginscan(node->ss.ss_currentRelation,
								   estate->es_snapshot,
								   0, NULL);
--table_scan_getnextslot
```