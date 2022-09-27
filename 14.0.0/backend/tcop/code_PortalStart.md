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
```