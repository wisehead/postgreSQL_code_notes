#1. MarkBufferDirty

```
MarkBufferDirty
--if (BufferIsLocal(buffer))
----MarkLocalBufferDirty(buffer);
--old_buf_state = pg_atomic_read_u32(&bufHdr->state);
--for (;;)
	{
		if (old_buf_state & BM_LOCKED)
			old_buf_state = WaitBufHdrUnlocked(bufHdr);

		buf_state = old_buf_state;

		Assert(BUF_STATE_GET_REFCOUNT(buf_state) > 0);
		buf_state |= BM_DIRTY | BM_JUST_DIRTIED;

		if (pg_atomic_compare_exchange_u32(&bufHdr->state, &old_buf_state,
										   buf_state))
			break;
	}
```