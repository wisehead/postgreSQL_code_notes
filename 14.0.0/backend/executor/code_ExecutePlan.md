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

```

#2.ExecReScan

```
ExecReScan
--if (node->chgParam != NULL)
----

```