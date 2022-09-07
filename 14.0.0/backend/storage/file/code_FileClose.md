#1.FileClose

```cpp
FileClose
--vfdP = &VfdCache[file];
--if (!FileIsNotOpen(file))
----close(vfdP->fd) 
---- --nfile;
----vfdP->fd = VFD_CLOSED;
----Delete(file);
------vfdP = &VfdCache[file];
------VfdCache[vfdP->lruLessRecently].lruMoreRecently = vfdP->lruMoreRecently;
------VfdCache[vfdP->lruMoreRecently].lruLessRecently = vfdP->lruLessRecently;
--if (vfdP->fdstate & FD_TEMP_FILE_LIMIT)
----temporary_files_size -= vfdP->fileSize;
----vfdP->fileSize = 0;
--if (vfdP->fdstate & FD_DELETE_AT_CLOSE)
----vfdP->fdstate &= ~FD_DELETE_AT_CLOSE;
----stat(vfdP->fileName, &filestats)
----unlink(vfdP->fileName)
----ReportTemporaryFileUsage
--if (vfdP->resowner)
----ResourceOwnerForgetFile(vfdP->resowner, file);
--FreeVfd(file);
```