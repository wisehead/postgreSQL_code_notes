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
```