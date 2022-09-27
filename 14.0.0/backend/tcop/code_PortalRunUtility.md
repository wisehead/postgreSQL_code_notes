#1.PortalRunUtility

```
PortalRunUtility
--if (PlannedStmtRequiresSnapshot(pstmt))
----snapshot = GetTransactionSnapshot();
----if (setHoldSnapshot)
------snapshot = RegisterSnapshot(snapshot);
------portal->holdSnapshot = snapshot;
----PushActiveSnapshot(snapshot);
----portal->portalSnapshot = GetActiveSnapshot();
--ProcessUtility(pstmt,
				   portal->sourceText,
				   (portal->cplan != NULL), /* protect tree if in plancache */
				   isTopLevel ? PROCESS_UTILITY_TOPLEVEL : PROCESS_UTILITY_QUERY,
				   portal->portalParams,
				   portal->queryEnv,
				   dest,
				   qc);
--if (portal->portalSnapshot != NULL && ActiveSnapshotSet())
----PopActiveSnapshot();				   
```