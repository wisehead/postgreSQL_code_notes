#1.shmem_exit

```
shmem_exit
--while (--before_shmem_exit_index >= 0)
----before_shmem_exit_list[before_shmem_exit_index].function(code, before_shmem_exit_list[before_shmem_exit_index].arg);
--dsm_backend_shutdown();
--while (--on_shmem_exit_index >= 0)
----on_shmem_exit_list[on_shmem_exit_index].function(code,
								on_shmem_exit_list[on_shmem_exit_index].arg);
```