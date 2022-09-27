#1.GetSnapshotData

```
GetSnapshotData
--if (snapshot->xip == NULL)
----snapshot->xip = (TransactionId *)
			malloc(GetMaxSnapshotXidCount() * sizeof(TransactionId));
--LWLockAcquire(ProcArrayLock, LW_SHARED);
--GetSnapshotDataReuse
--if (!snapshot->takenDuringRecovery)
----for (int pgxactoff = 0; pgxactoff < numProcs; pgxactoff++)
------
--else//(!snapshot->takenDuringRecovery)
----subcount = KnownAssignedXidsGetAndSetXmin(snapshot->subxip, &xmin,
												  xmax);
--LWLockRelease(ProcArrayLock);
```

#2. GetSnapshotDataReuse

```
GetSnapshotDataReuse 
--curXactCompletionCount = ShmemVariableCache->xactCompletionCount;
--if (curXactCompletionCount != snapshot->snapXactCompletionCount)
----return false;                                   
--GetSnapshotDataInitOldSnapshot                        
----snapshot->lsn = GetXLogInsertRecPtr();              
------current_bytepos = Insert->CurrBytePos;            
----snapshot->whenTaken = GetSnapshotCurrentTimestamp();
----MaintainOldSnapshotTimeMapping                      

```