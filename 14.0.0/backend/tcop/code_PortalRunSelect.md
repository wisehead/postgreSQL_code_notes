#1.PortalRunSelect

```
PortalRunSelect
--if (forward)
----if (portal->holdStore)
------RunFromStore(portal, direction, (uint64) count, dest);
----else
------PushActiveSnapshot(queryDesc->snapshot);         
------ExecutorRun(queryDesc, direction, (uint64) count,
				portal->run_once);                         
------nprocessed = queryDesc->estate->es_processed;    
------PopActiveSnapshot();                             

```