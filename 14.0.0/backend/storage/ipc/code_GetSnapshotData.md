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
--if (!snapshot->takenDuringRecovery)
----for (int pgxactoff = 0; pgxactoff < numProcs; pgxactoff++)
------
--else//(!snapshot->takenDuringRecovery)
----subcount = KnownAssignedXidsGetAndSetXmin(snapshot->subxip, &xmin,
												  xmax);
--LWLockRelease(ProcArrayLock);
```