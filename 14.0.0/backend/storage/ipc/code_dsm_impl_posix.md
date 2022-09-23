#1. dsm_impl_posix

```
dsm_impl_posix
--snprintf(name, 64, "/PostgreSQL.%u", handle);
--if (op == DSM_OP_DETACH || op == DSM_OP_DESTROY)
----if (*mapped_address != NULL
			&& munmap(*mapped_address, *mapped_size) != 0)
------return false;
----if (op == DSM_OP_DESTROY && shm_unlink(name) != 0)
------return false;
----return true;
--ReserveExternalFD
----ReleaseLruFiles
--flags = O_RDWR | (op == DSM_OP_CREATE ? O_CREAT | O_EXCL : 0);
--fd = shm_open(name, flags, PG_FILE_MODE_OWNER))
--if (op == DSM_OP_ATTACH)
----fstat(fd, &st)
----request_size = st.st_size;
--else if (dsm_impl_posix_resize(fd, request_size) != 0)
----return false;
--address = mmap(NULL, request_size, PROT_READ | PROT_WRITE,
				   MAP_SHARED | MAP_HASSEMAPHORE | MAP_NOSYNC, fd, 0);
--*mapped_address = address;   
--*mapped_size = request_size; 
--close(fd);                   
--ReleaseExternalFD();         
--return true
```



#2.comments

```cpp
/*
 * Operating system primitives to support POSIX shared memory.
 *
 * POSIX shared memory segments are created and attached using shm_open()
 * and shm_unlink(); other operations, such as sizing or mapping the
 * segment, are performed as if the shared memory segments were files.
 *
 * Indeed, on some platforms, they may be implemented that way.  While
 * POSIX shared memory segments seem intended to exist in a flat namespace,
 * some operating systems may implement them as files, even going so far
 * to treat a request for /xyz as a request to create a file by that name
 * in the root directory.  Users of such broken platforms should select
 * a different shared memory implementation.
 */
```

#3. dsm_impl_posix_resize

```
dsm_impl_posix_resize
--
```