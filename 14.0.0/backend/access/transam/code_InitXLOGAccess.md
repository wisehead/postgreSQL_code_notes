#1.InitXLOGAccess

```
InitXLOGAccess
--XLogCtlInsert *Insert = &XLogCtl->Insert;
--ThisTimeLineID = XLogCtl->ThisTimeLineID;
--wal_segment_size = ControlFile->xlog_seg_size;
--GetRedoRecPtr
--doPageWrites = (Insert->fullPageWrites || Insert->forcePageWrites);
--InitXLogInsert
```

#2.GetRedoRecPtr

```
GetRedoRecPtr
--SpinLockAcquire(&XLogCtl->info_lck);
--ptr = XLogCtl->RedoRecPtr;
--SpinLockRelease(&XLogCtl->info_lck);
```