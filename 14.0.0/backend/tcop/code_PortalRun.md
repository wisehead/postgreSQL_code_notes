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
------PortalRunSelect
------if (qc && portal->qc.commandTag != CMDTAG_UNKNOWN)
--------CopyQueryCompletion(qc, &portal->qc);
--------qc->nprocessed = nprocessed;
--------portal->status = PORTAL_READY;
--------result = portal->atEnd;
--------break;
----case PORTAL_MULTI_QUERY:
------PortalRunMulti(portal, isTopLevel, false,
							   dest, altdest, qc);
------MarkPortalDone(portal);
------result = true;
------break;


```