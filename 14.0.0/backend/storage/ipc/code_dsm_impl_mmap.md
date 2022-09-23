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
--if (op == DSM_OP_ATTACH)
----fstat(fd, &st) 
----request_size = st.st_size;
--else
----while (success && remaining > 0)
		{
			Size		goal = remaining;

			if (goal > ZBUFFER_SIZE)
				goal = ZBUFFER_SIZE;
			pgstat_report_wait_start(WAIT_EVENT_DSM_FILL_ZERO_WRITE);
			if (write(fd, zbuffer, goal) == goal)
				remaining -= goal;
			else
				success = false;
			pgstat_report_wait_end();
		}
----//end while
--address = mmap(NULL, request_size, PROT_READ | PROT_WRITE,
				   MAP_SHARED | MAP_HASSEMAPHORE | MAP_NOSYNC, fd, 0);
--*mapped_address = address;
--*mapped_size = request_size;
--CloseTransientFile(fd) 
--return true;
```