#1.PortalDrop

```
PortalDrop
--if (PointerIsValid(portal->cleanup))
----portal->cleanup(portal);
----portal->cleanup = NULL;
--PortalReleaseCachedPlan
----ReleaseCachedPlan
--if (portal->holdSnapshot)
----if (portal->resowner)
------UnregisterSnapshotFromOwner(portal->holdSnapshot,
										portal->resowner);
--if (portal->holdStore)
----tuplestore_end(portal->holdStore);

```