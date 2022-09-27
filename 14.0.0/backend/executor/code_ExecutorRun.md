#1.ExecutorRun

```
ExecutorRun
--standard_ExecutorRun(queryDesc, direction, count, execute_once);
```

#2.standard_ExecutorRun

```
standard_ExecutorRun
--if (queryDesc->totaltime)
----InstrStartNode(queryDesc->totaltime);
--sendTuples = (operation == CMD_SELECT ||
				  queryDesc->plannedstmt->hasReturning);
--if (sendTuples)
----dest->rStartup(dest, operation, queryDesc->tupDesc);
------tstoreStartupReceiver
```