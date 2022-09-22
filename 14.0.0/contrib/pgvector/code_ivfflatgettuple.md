#1. ivfflatgettuple

```cpp
ivfflatgettuple
--if (so->first)
--//not first time
--tuplesort_gettupleslot(so->sortstate, true, false, so->slot, NULL)
--ItemPointer tid = (ItemPointer) DatumGetPointer(slot_getattr(so->slot, 2, &so->isnull));
--BlockNumber indexblkno = DatumGetInt32(slot_getattr(so->slot, 3, &so->isnull));
--scan->xs_heaptid = *tid;
--ReleaseBuffer(so->buf);
----UnpinBuffer(GetBufferDescriptor(buffer - 1), true);
--ReadBuffer
----ReadBufferExtended
```