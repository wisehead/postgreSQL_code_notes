#1.OpenTemporaryFile

```
OpenTemporaryFile
--ResourceOwnerEnlargeFiles
--if (numTempTableSpaces > 0 && !interXact)
----tblspcOid = GetNextTempTableSpace();
----file = OpenTemporaryFileInTablespace(tblspcOid, false);
```