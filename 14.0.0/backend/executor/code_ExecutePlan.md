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
----if (sendTuples)
------dest->receiveSlot(slot, dest)
--------tstoreReceiveSlot_notoast
----------tuplestore_puttupleslot
------------tuple = ExecCopySlotMinimalTuple(slot);
------------tuplestore_puttuple_common(state, (void *) tuple);
--if (!(estate->es_top_eflags & EXEC_FLAG_BACKWARD))
----(void) ExecShutdownNode(planstate);

--if (use_parallel_mode)
----ExitParallelMode();
```

