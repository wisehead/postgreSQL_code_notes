#1.AddToDataDirLockFile

```
AddToDataDirLockFile
--fd = open(DIRECTORY_LOCK_FILE, O_RDWR | PG_BINARY, 0);
--len = read(fd, srcbuffer, sizeof(srcbuffer) - 1);
```