#1.ExecutePlan

```
ExecutePlan
--if (use_parallel_mode)
----EnterParallelMode();
--for (;;)
----slot = ExecProcNode(planstate);
------if (node->chgParam != NULL) /
--------ExecReScan(node);
------node->ExecProcNode(node);//每个node的handler不一样
----if (estate->es_junkFilter != NULL)
------slot = ExecFilterJunk(estate->es_junkFilter, slot);

```

