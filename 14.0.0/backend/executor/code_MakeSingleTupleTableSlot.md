#1.MakeSingleTupleTableSlot

```
MakeSingleTupleTableSlot
-- MakeTupleTableSlot(tupdesc, tts_ops);
```

#2. MakeTupleTableSlot

```
MakeTupleTableSlot
--basesz = tts_ops->base_slot_size;
--*((const TupleTableSlotOps **) &slot->tts_ops) = tts_ops;
--slot->tts_ops->init(slot);

```