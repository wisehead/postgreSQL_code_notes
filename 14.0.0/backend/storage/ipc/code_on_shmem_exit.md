#1.on_shmem_exit

```
on_shmem_exit
--on_proc_exit_list[on_proc_exit_index].function = function;
--on_proc_exit_list[on_proc_exit_index].arg = arg;
--++on_proc_exit_index;
--if (!atexit_callback_setup)
----atexit(atexit_callback);
----atexit_callback_setup = true;
```