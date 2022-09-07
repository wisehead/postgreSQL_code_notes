#1.GetSnapshotData

```
GetSnapshotData
--LWLockAcquire(ProcArrayLock, LW_SHARED);
--GetSnapshotDataReuse
----GetSnapshotDataInitOldSnapshot
------snapshot->lsn = GetXLogInsertRecPtr();
--------current_bytepos = Insert->CurrBytePos;
------snapshot->whenTaken = GetSnapshotCurrentTimestamp();
------MaintainOldSnapshotTimeMapping
```