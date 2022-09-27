#1.FillPortalStore

```
FillPortalStore
--PortalCreateHoldStore(portal);
----portal->holdStore =
		tuplestore_begin_heap(portal->cursorOptions & CURSOR_OPT_SCROLL,
							  true, work_mem);
------tuplestore_begin_common
```