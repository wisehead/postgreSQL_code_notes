#1.OpenTemporaryFile

```
OpenTemporaryFile
--ResourceOwnerEnlargeFiles
--if (numTempTableSpaces > 0 && !interXact)
----tblspcOid = GetNextTempTableSpace();
----file = OpenTemporaryFileInTablespace(tblspcOid, false);
--VfdCache[file].fdstate |= FD_DELETE_AT_CLOSE | FD_TEMP_FILE_LIMIT;
--if (!interXact)
----RegisterTemporaryFile(file);
------VfdCache[file].resowner = CurrentResourceOwner;
------VfdCache[file].fdstate |= FD_CLOSE_AT_EOXACT;	
```