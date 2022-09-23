#1.dsm_impl_sysv

```
dsm_impl_sysv
--snprintf(name, 64, "%u", handle);
--key = (key_t) handle;
--if (*impl_private != NULL)
----ident_cache = *impl_private;
----ident = *ident_cache;
--else
----ident_cache = MemoryContextAlloc(TopMemoryContext, sizeof(int));
----if (op == DSM_OP_CREATE)
------flags |= IPC_CREAT | IPC_EXCL;
------segsize = request_size;
----ident = shmget(key, segsize, flags)
----*ident_cache = ident;
----*impl_private = ident_cache;
--if (op == DSM_OP_DETACH || op == DSM_OP_DESTROY)
----if (*mapped_address != NULL && shmdt(*mapped_address) != 0)
------return false;
----if (op == DSM_OP_DESTROY && shmctl(ident, IPC_RMID, NULL) < 0)
------return false;
----return true;
--if (op == DSM_OP_ATTACH)
----shmctl(ident, IPC_STAT, &shm)
----request_size = shm.shm_segsz;
--address = shmat(ident, NULL, PG_SHMAT_FLAGS);
--*mapped_address = address;
--*mapped_size = request_size;
--return true
```