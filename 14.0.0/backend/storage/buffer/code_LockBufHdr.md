#1.LockBufHdr

```
LockBufHdr
--init_local_spin_delay(&delayStatus);
--while (true)
--{
		/* set BM_LOCKED flag */
		old_buf_state = pg_atomic_fetch_or_u32(&desc->state, BM_LOCKED);
		/* if it wasn't set before we're OK */
		if (!(old_buf_state & BM_LOCKED))
			break;
		perform_spin_delay(&delayStatus);
--}
--finish_spin_delay(&delayStatus);
```