#1. dsm_impl_posix

```
dsm_impl_posix
--snprintf(name, 64, "/PostgreSQL.%u", handle);
--
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