#1.SetLatch

```
SetLatch
--pg_memory_barrier();
--if (latch->is_set)
----return;
--latch->is_set = true;
--pg_memory_barrier();
--if (!latch->maybe_sleeping)
----return;
--owner_pid = latch->owner_pid;
--if (owner_pid == 0)
----return;
--else if (owner_pid == MyProcPid)
----if (waiting)
------kill(MyProcPid, SIGURG);
--else
----kill(owner_pid, SIGURG);
```