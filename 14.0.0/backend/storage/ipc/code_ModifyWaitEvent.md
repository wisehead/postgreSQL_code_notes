#1.ModifyWaitEvent

```
ModifyWaitEvent
--event = &set->events[pos];
--set->latch = latch;
--WaitEventAdjustEpoll(set, event, EPOLL_CTL_MOD);
----epoll_ev.data.ptr = event;
----epoll_ev.events = EPOLLERR | EPOLLHUP;
----if (event->events == WL_LATCH_SET)
------epoll_ev.events |= EPOLLIN;
----else if (event->events == WL_POSTMASTER_DEATH)
------epoll_ev.events |= EPOLLIN;
----else
------if (event->events & WL_SOCKET_READABLE)
			epoll_ev.events |= EPOLLIN;
------if (event->events & WL_SOCKET_WRITEABLE)
			epoll_ev.events |= EPOLLOUT;
----epoll_ctl(set->epoll_fd, action, event->fd, &epoll_ev);
```