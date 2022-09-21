#1.WaitEventSetWait

```
WaitEventSetWait(WaitEventSet *set, long timeout,
				 WaitEvent *occurred_events, int nevents,
				 uint32 wait_event_info)
--waiting = true;
--while (returned_events == 0)
----if (set->latch && !set->latch->is_set)//没有Signal到来，没有调用SetLatch
------/* about to sleep on a latch */
------set->latch->maybe_sleeping = true;
----if (set->latch && set->latch->is_set)
------occurred_events->fd = PGINVALID_SOCKET;                              
------occurred_events->pos = set->latch_pos;                               
------occurred_events->user_data = set->events[set->latch_pos].user_data;  
------occurred_events->events = WL_LATCH_SET;                              
------occurred_events++;                                                   
------returned_events++;                                                   
------/* could have been set above */                                      
------set->latch->maybe_sleeping = false;                                  
------break;                                                               
----WaitEventSetWaitBlock(set, cur_timeout,
								   occurred_events, nevents);
----if (set->latch)
------set->latch->maybe_sleeping = false;
--waiting = false;
--return returned_events;
```

#2.caller

```
WaitLatch
--ModifyWaitEvent(LatchWaitSet, LatchWaitSetLatchPos, WL_LATCH_SET, latch);
--WaitEventSetWait(LatchWaitSet,
						 (wakeEvents & WL_TIMEOUT) ? timeout : -1,
						 &event, 1,
						 wait_event_info)
```