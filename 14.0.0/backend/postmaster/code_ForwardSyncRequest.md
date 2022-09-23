#1.ForwardSyncRequest

```
ForwardSyncRequest
--LWLockAcquire(CheckpointerCommLock, LW_EXCLUSIVE);
--if (CheckpointerShmem->checkpointer_pid == 0 ||
		(CheckpointerShmem->num_requests >= CheckpointerShmem->max_requests &&
		 !CompactCheckpointerRequestQueue()))
----LWLockRelease(CheckpointerCommLock);
----return false;
--request = &CheckpointerShmem->requests[CheckpointerShmem->num_requests++];
--request->ftag = *ftag;
--request->type = type;	 
--LWLockRelease(CheckpointerCommLock);
--if (too_full && ProcGlobal->checkpointerLatch)
----SetLatch(ProcGlobal->checkpointerLatch);
--return true;
```

#2.CompactCheckpointerRequestQueue

```
CompactCheckpointerRequestQueue
--
```