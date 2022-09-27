#1.tstoreStartupReceiver

```
tstoreStartupReceiver
--if (myState->target_tupdesc)
----myState->tupmap = convert_tuples_by_position(typeinfo,
		myState->target_tupdesc,
		myState->map_failure_msg);
--if (needtoast)
----myState->pub.receiveSlot = tstoreReceiveSlot_detoast;
--else if (myState->tupmap)
----myState->pub.receiveSlot = tstoreReceiveSlot_tupmap;
----myState->mapslot = MakeSingleTupleTableSlot(myState->target_tupdesc,					&TTSOpsVirtual);
--else
----myState->pub.receiveSlot = tstoreReceiveSlot_notoast;
```