#1. PortalRun

```
PortalRun
--MarkPortalActive
----portal->status = PORTAL_ACTIVE;
----portal->activeSubid = GetCurrentSubTransactionId();
--switch (portal->strategy)
----case PORTAL_ONE_SELECT:
			case PORTAL_ONE_RETURNING:
			case PORTAL_ONE_MOD_WITH:
			case PORTAL_UTIL_SELECT:
------if (portal->strategy != PORTAL_ONE_SELECT && !portal->holdStore)
--------FillPortalStore(portal, isTopLevel);
```