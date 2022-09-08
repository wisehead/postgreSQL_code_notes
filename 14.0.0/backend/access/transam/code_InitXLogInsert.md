#1.InitXLogInsert

```
InitXLogInsert
--xloginsert_cxt = AllocSetContextCreate
----AllocSetContextCreateInternal
--registered_buffers = MemoryContextAllocZero
----context->methods->alloc(context, size);
----MemSetAligned(ret, 0, size);
--rdatas = MemoryContextAlloc
--hdr_scratch = MemoryContextAllocZero
```