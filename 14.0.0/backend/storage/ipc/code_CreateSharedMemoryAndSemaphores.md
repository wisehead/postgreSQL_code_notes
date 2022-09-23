#1.CreateSharedMemoryAndSemaphores

```
CreateSharedMemoryAndSemaphores
--if (!IsUnderPostmaster)
----numSemas = ProcGlobalSemas();
----numSemas += SpinlockSemas();
----size = add_size(size, total_addin_request);
----size = add_size(size, 8192 - (size % 8192));
----seghdr = PGSharedMemoryCreate(size, &shim);
----InitShmemAccess(seghdr);
----PGReserveSemaphores(numSemas);
```

//to be continued