#1.dsm_impl_op

```
dsm_impl_op
--switch (dynamic_shared_memory_type)
----case DSM_IMPL_POSIX:
------dsm_impl_posix(op, handle, request_size, impl_private,
								  mapped_address, mapped_size, elevel);
----case DSM_IMPL_SYSV:
------return dsm_impl_sysv(op, handle, request_size, impl_private,
								 mapped_address, mapped_size, elevel);
----case DSM_IMPL_MMAP:
------return dsm_impl_mmap(op, handle, request_size, impl_private,
								 mapped_address, mapped_size, elevel);
```
