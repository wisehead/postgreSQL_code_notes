#1.GetSnapshotData
//to be continued
```
GetSnapshotData
--if (snapshot->xip == NULL)
----snapshot->xip = (TransactionId *)
			malloc(GetMaxSnapshotXidCount() * sizeof(TransactionId));
--LWLockAcquire(ProcArrayLock, LW_SHARED);
--if (GetSnapshotDataReuse(snapshot))
----return snapshot;
--if (!snapshot->takenDuringRecovery)
----for (int pgxactoff = 0; pgxactoff < numProcs; pgxactoff++)
------TransactionId xid = UINT32_ACCESS_ONCE(other_xids[pgxactoff]);
------xip[count++] = xid;
------int			nsubxids = subxidStates[pgxactoff].count;
------if (nsubxids > 0)
--------int			pgprocno = pgprocnos[pgxactoff];
--------PGPROC	   *proc = &allProcs[pgprocno];
--------memcpy(snapshot->subxip + subcount,
							   (void *) proc->subxids.xids,
							   nsubxids * sizeof(TransactionId));
--------subcount += nsubxids;
--else//(!snapshot->takenDuringRecovery)
----subcount = KnownAssignedXidsGetAndSetXmin(snapshot->subxip, &xmin,
												  xmax);
--LWLockRelease(ProcArrayLock);
--oldestfxid = FullXidRelativeTo(latest_completed, oldestxid);
--def_vis_xid_data =
			TransactionIdRetreatedBy(xmin, vacuum_defer_cleanup_age);

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