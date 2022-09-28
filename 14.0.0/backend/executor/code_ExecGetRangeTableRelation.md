#1.ExecGetRangeTableRelation

```
ExecGetRangeTableRelation
--rel = estate->es_relations[rti - 1];
--RangeTblEntry *rte = exec_rt_fetch(rti, estate);
--table_open
```