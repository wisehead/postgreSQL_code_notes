#1.struct WaitEventSet

```cpp
/* typedef in latch.h */
struct WaitEventSet
{
	int			nevents;		/* number of registered events */
	int			nevents_space;	/* maximum number of events in this set */

	/*
	 * Array, of nevents_space length, storing the definition of events this
	 * set is waiting for.
	 */
	WaitEvent  *events;

	/*
	 * If WL_LATCH_SET is specified in any wait event, latch is a pointer to
	 * said latch, and latch_pos the offset in the ->events array. This is
	 * useful because we check the state of the latch before performing doing
	 * syscalls related to waiting.
	 */
	Latch	   *latch;
	int			latch_pos;

	/*
	 * WL_EXIT_ON_PM_DEATH is converted to WL_POSTMASTER_DEATH, but this flag
	 * is set so that we'll exit immediately if postmaster death is detected,
	 * instead of returning.
	 */
	bool		exit_on_postmaster_death;

#if defined(WAIT_USE_EPOLL)
	int			epoll_fd;
	/* epoll_wait returns events in a user provided arrays, allocate once */
	struct epoll_event *epoll_ret_events;
#elif defined(WAIT_USE_KQUEUE)
	int			kqueue_fd;
	/* kevent returns events in a user provided arrays, allocate once */
	struct kevent *kqueue_ret_events;
	bool		report_postmaster_not_running;
#elif defined(WAIT_USE_POLL)
	/* poll expects events to be waited on every poll() call, prepare once */
	struct pollfd *pollfds;
#elif defined(WAIT_USE_WIN32)

	/*
	 * Array of windows events. The first element always contains
	 * pgwin32_signal_event, so the remaining elements are offset by one (i.e.
	 * event->pos + 1).
	 */
	HANDLE	   *handles;
#endif
};

```