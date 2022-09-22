#1.GetRelationPath

```
GetRelationPath
--if (spcNode == GLOBALTABLESPACE_OID)
----path = psprintf("global/%u", relNode);
--else if (spcNode == DEFAULTTABLESPACE_OID)
----if (backendId == InvalidBackendId)
------psprintf("base/%u/%u", dbNode, relNode);
----else
------psprintf("base/%u/t%d_%u",dbNode, backendId, relNode);
--else
----if (backendId == InvalidBackendId)
------psprintf("pg_tblspc/%u/%s/%u/%u",
								spcNode, TABLESPACE_VERSION_DIRECTORY,
								dbNode, relNode);
----else
------psprintf("pg_tblspc/%u/%s/%u/t%d_%u",
								spcNode, TABLESPACE_VERSION_DIRECTORY,
								dbNode, backendId, relNode);
```