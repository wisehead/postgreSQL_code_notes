#1.UnpinBuffer

```
UnpinBuffer
--GetPrivateRefCountEntry
--ref->refcount--;
--if (ref->refcount == 0)
----old_buf_state = pg_atomic_read_u32(&buf->state);
----for (;;)
		{
			if (old_buf_state & BM_LOCKED)
				old_buf_state = WaitBufHdrUnlocked(buf);

			buf_state = old_buf_state;

			buf_state -= BUF_REFCOUNT_ONE;

			if (pg_atomic_compare_exchange_u32(&buf->state, &old_buf_state,
											   buf_state))
				break;
----}
----if (buf_state & BM_PIN_COUNT_WAITER)
------buf_state = LockBufHdr(buf);
------if ((buf_state & BM_PIN_COUNT_WAITER) &&
				BUF_STATE_GET_REFCOUNT(buf_state) == 1)
------int			wait_backend_pid = buf->wait_backend_pid;
------buf_state &= ~BM_PIN_COUNT_WAITER;
------UnlockBufHdr(buf, buf_state);
------ProcSendSignal(wait_backend_pid);
----else
------UnlockBufHdr(buf, buf_state);
--ForgetPrivateRefCountEntry(ref);
```