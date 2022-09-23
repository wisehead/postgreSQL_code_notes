#1.PinBuffer

```
PinBuffer
--b = BufferDescriptorGetBuffer(buf);
--GetPrivateRefCountEntry(b, true);
--if (ref == NULL)
----ReservePrivateRefCountEntry();
----ref = NewPrivateRefCountEntry(b);
----for (;;)
------if (old_buf_state & BM_LOCKED)
--------old_buf_state = WaitBufHdrUnlocked(buf);
------buf_state = old_buf_state;
------buf_state += BUF_REFCOUNT_ONE;
------pg_atomic_compare_exchange_u32(&buf->state, &old_buf_state,
											   buf_state))
--else
----result = true;
--ref->refcount++;
```