#1.InternalIpcMemoryCreate

```
InternalIpcMemoryCreate
--shmid = shmget(memKey, size, IPC_CREAT | IPC_EXCL | IPCProtection);
--on_shmem_exit(IpcMemoryDelete, Int32GetDatum(shmid));
--memAddress = shmat(shmid, requestedAddress, PG_SHMAT_FLAGS);
--on_shmem_exit(IpcMemoryDetach, PointerGetDatum(memAddress));
--sprintf(line, "%9lu %9lu",
				(unsigned long) memKey, (unsigned long) shmid);
--AddToDataDirLockFile(LOCK_FILE_LINE_SHMEM_KEY, line);
```

#2.IpcMemoryDelete

```
IpcMemoryDelete
--shmctl(DatumGetInt32(shmId), IPC_RMID, NULL) 
```

#3.IpcMemoryDetach

```
IpcMemoryDetach
--shmdt(DatumGetPointer(shmaddr))
```