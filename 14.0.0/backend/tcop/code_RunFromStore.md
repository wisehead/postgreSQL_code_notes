#1.RunFromStore

```
RunFromStore
--MakeSingleTupleTableSlot
--dest->rStartup(dest, CMD_SELECT, portal->tupDesc);
--for (;;)
----tuplestore_gettupleslot
----ExecClearTuple(slot);
--ExecDropSingleTupleTableSlot(slot);
```