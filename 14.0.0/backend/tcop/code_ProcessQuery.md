#1.ProcessQuery

```
ProcessQuery
--CreateQueryDesc
--ExecutorStart(queryDesc, 0);
--ExecutorRun(queryDesc, ForwardScanDirection, 0L, true);
----standard_ExecutorRun
--if (qc)
----switch (queryDesc->operation)
------case CMD_SELECT:
--------SetQueryCompletion
--------break;
------case CMD_INSERT:
--------SetQueryCompletion
--------break;
------case CMD_UPDATE:
--------SetQueryCompletion
--------break;
------case CMD_DELETE:
--------SetQueryCompletion
--------break;
------default:
--------SetQueryCompletion
--------break;
--ExecutorFinish(queryDesc);
----standard_ExecutorFinish
------ExecPostprocessPlan
--------foreach(lc, estate->es_auxmodifytables)
----------for (;;)
------------slot = ExecProcNode(ps);
--ExecutorEnd(queryDesc);
----standard_ExecutorEnd
------ExecEndPlan(queryDesc->planstate, estate);
--FreeQueryDesc(queryDesc);
```