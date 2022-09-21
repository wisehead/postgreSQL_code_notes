#1.WaitEventSetWaitBlock

```
WaitEventSetWaitBlock(WaitEventSet *set, int cur_timeout,
					  WaitEvent *occurred_events, int nevents)
--rc = epoll_wait(set->epoll_fd, set->epoll_ret_events,
					nevents, cur_timeout);
--for (cur_epoll_event = set->epoll_ret_events;
		 cur_epoll_event < (set->epoll_ret_events + rc) &&
		 returned_events < nevents;
		 cur_epoll_event++)
----cur_event = (WaitEvent *) cur_epoll_event->data.ptr;                                                  
----occurred_events->pos = cur_event->pos;               
----occurred_events->user_data = cur_event->user_data;   
----occurred_events->events = 0;                         
----if (cur_event->events == WL_LATCH_SET &&
			cur_epoll_event->events & (EPOLLIN | EPOLLERR | EPOLLHUP))
------drain
------if (set->latch && set->latch->is_set)
--------occurred_events->fd = PGINVALID_SOCKET;   
--------occurred_events->events = WL_LATCH_SET;   
--------occurred_events++;                        
--------returned_events++;                        
------else//other events
--return returned_events;
```