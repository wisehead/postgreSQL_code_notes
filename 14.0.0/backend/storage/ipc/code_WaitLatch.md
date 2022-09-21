#1.WaitLatch

```
WaitLatch
--ModifyWaitEvent(LatchWaitSet, LatchWaitSetLatchPos, WL_LATCH_SET, latch);
--WaitEventSetWait(LatchWaitSet,
						 (wakeEvents & WL_TIMEOUT) ? timeout : -1,
						 &event, 1,
						 wait_event_info)
--return event.events;
```