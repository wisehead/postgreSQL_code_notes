#1.ProcSendSignal

```
ProcSendSignal
--RecoveryInProgress
--SpinLockAcquire(ProcStructLock);
--if (pid == ProcGlobal->startupProcPid)
----proc = ProcGlobal->startupProc;
--SpinLockRelease(ProcStructLock);
--if (proc == NULL)
----proc = BackendPidGetProc(pid);

--if (proc != NULL)
----SetLatch(&proc->procLatch);//
```