#1.dsm_impl_mmap

```
dsm_impl_mmap
--snprintf(name, 64, PG_DYNSHMEM_DIR "/" PG_DYNSHMEM_MMAP_FILE_PREFIX "%u",
			 handle);
--if (op == DSM_OP_DETACH || op == DSM_OP_DESTROY)
----if (*mapped_address != NULL
			&& munmap(*mapped_address, *mapped_size) != 0)
------return false;
----if (op == DSM_OP_DESTROY && unlink(name) != 0)
------return false;
----return true;
--flags = O_RDWR | (op == DSM_OP_CREATE ? O_CREAT | O_EXCL : 0);
--fd = OpenTransientFile(name, flags))
----OpenTransientFilePerm
```