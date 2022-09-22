#1.PGSharedMemoryCreate

```
PGSharedMemoryCreate
--stat(DataDir, &statbuf)
--NextShmemSegID = statbuf.st_ino;
--for (;;)
----memAddress = InternalIpcMemoryCreate(NextShmemSegID, sysvsize);
```