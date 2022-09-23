#1.LWLockConditionalAcquire

```
LWLockConditionalAcquire
--HOLD_INTERRUPTS();
--mustwait = LWLockAttemptLock(lock, mode);
--if (mustwait)
----RESUME_INTERRUPTS
--else
----held_lwlocks[num_held_lwlocks].lock = lock;
----held_lwlocks[num_held_lwlocks++].mode = mode;
--return !mustwait;
```