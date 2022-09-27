#1.FillPortalStore

```
FillPortalStore
--PortalCreateHoldStore(portal);
----portal->holdStore =
		tuplestore_begin_heap(portal->cursorOptions & CURSOR_OPT_SCROLL,
							  true, work_mem);
------tuplestore_begin_common
--CreateDestReceiver
----CreateTuplestoreDestReceiver 
--SetTuplestoreDestReceiverParams(treceiver,
									portal->holdStore,
									portal->holdContext,
									false,
									NULL,
									NULL);
--switch (portal->strategy)
----case PORTAL_ONE_RETURNING:
	 case PORTAL_ONE_MOD_WITH:				
------PortalRunMulti(portal, isTopLevel, true,
						   treceiver, None_Receiver, &qc);
------break;	 		
----case PORTAL_UTIL_SELECT:
------PortalRunUtility(portal, linitial_node(PlannedStmt, portal->stmts),
							 isTopLevel, true, treceiver, &qc);
------break;		
----default:	
------break;	
--if (qc.commandTag != CMDTAG_UNKNOWN)
-------CopyQueryCompletion(&portal->qc, &qc);
--treceiver->rDestroy(treceiver);
```