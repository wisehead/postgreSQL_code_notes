#1.BackendPidGetProc

```
BackendPidGetProc
--LWLockAcquire(ProcArrayLock, LW_SHARED);
--BackendPidGetProcWithLock(pid);
--LWLockRelease(ProcArrayLock);
```