#1.PinBuffer_Locked

```
PinBuffer_Locked
--buf_state = pg_atomic_read_u32(&buf->state);
--buf_state += BUF_REFCOUNT_ONE;
--UnlockBufHdr(buf, buf_state);
--b = BufferDescriptorGetBuffer(buf);
--ref = NewPrivateRefCountEntry(b);
--ref->refcount++;
```