#1. PushActiveSnapshot

```
PushActiveSnapshot
--if (snap == CurrentSnapshot || snap == SecondarySnapshot || !snap->copied)
----newactive->as_snap = CopySnapshot(snap);
--else
----newactive->as_snap = snap;
--newactive->as_next = ActiveSnapshot;
--newactive->as_level = GetCurrentTransactionNestLevel();
```