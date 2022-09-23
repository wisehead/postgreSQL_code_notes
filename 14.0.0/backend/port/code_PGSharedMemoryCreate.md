#1.PGSharedMemoryCreate

```
PGSharedMemoryCreate
--stat(DataDir, &statbuf)
--NextShmemSegID = statbuf.st_ino;
--for (;;)
----memAddress = InternalIpcMemoryCreate(NextShmemSegID, sysvsize);
----shmid = shmget(NextShmemSegID, sizeof(PGShmemHeader), 0);
----PGSharedMemoryAttach
```