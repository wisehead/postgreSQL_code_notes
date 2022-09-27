#1.PortalRunMulti

```
PortalRunMulti
--foreach(stmtlist_item, portal->stmts)
----PlannedStmt *pstmt = lfirst_node(PlannedStmt, stmtlist_item);
----if (pstmt->utilityStmt == NULL)
------if (!active_snapshot_set)
--------snapshot = GetTransactionSnapshot();
--------if (setHoldSnapshot)
----------napshot = RegisterSnapshot(snapshot);
----------portal->holdSnapshot = snapshot;
------else
--------UpdateActiveSnapshotCommandId();
------if (pstmt->canSetTag)
--------ProcessQuery(pstmt,
							 portal->sourceText,
							 portal->portalParams,
							 portal->queryEnv,
							 dest, qc);
------else
--------ProcessQuery(pstmt,
							 portal->sourceText,
							 portal->portalParams,
							 portal->queryEnv,
							 altdest, NULL);
----else
------if (pstmt->canSetTag)
--------PortalRunUtility(portal, pstmt, isTopLevel, false,
								 dest, qc);
------else
--------PortalRunUtility(portal, pstmt, isTopLevel, false,
								 altdest, NULL);
--if (active_snapshot_set)
----PopActiveSnapshot();					
--if (qc && qc->commandTag == CMDTAG_UNKNOWN)
----if (portal->qc.commandTag != CMDTAG_UNKNOWN)
------CopyQueryCompletion(qc, &portal->qc);
```