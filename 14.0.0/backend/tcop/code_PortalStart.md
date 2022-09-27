#1.PortalStart

```
PortalStart
--PG_TRY();
--portal->strategy = ChoosePortalStrategy(portal->stmts);
--switch (portal->strategy)
----case PORTAL_ONE_SELECT:
------if (snapshot)
--------PushActiveSnapshot(snapshot);
------else
--------PushActiveSnapshot(GetTransactionSnapshot());
------queryDesc = CreateQueryDesc(linitial_node(PlannedStmt, portal->stmts),
											portal->sourceText,
											GetActiveSnapshot(),
											InvalidSnapshot,
											None_Receiver,
											params,
											portal->queryEnv,
											0);
------ExecutorStart(queryDesc, myeflags);
------PopActiveSnapshot();
------break;
----case PORTAL_ONE_RETURNING:
----case PORTAL_ONE_MOD_WITH:
------pstmt = PortalGetPrimaryStmt(portal);
------portal->tupDesc =
						ExecCleanTypeFromTL(pstmt->planTree->targetlist);
------break;
----case PORTAL_UTIL_SELECT:
------PlannedStmt *pstmt = PortalGetPrimaryStmt(portal);
------portal->tupDesc = UtilityTupleDescriptor(pstmt->utilityStmt);
------break;
----case PORTAL_MULTI_QUERY:
				/* Need do nothing now */
------portal->tupDesc = NULL;
------break;
```