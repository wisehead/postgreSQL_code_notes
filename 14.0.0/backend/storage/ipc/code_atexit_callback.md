#1.atexit_callback

```
atexit_callback
--proc_exit_prepare
```

#2.proc_exit_prepare

```
proc_exit_prepare
--shmem_exit(code);
--while (--on_proc_exit_index >= 0)
		on_proc_exit_list[on_proc_exit_index].function(code,
			on_proc_exit_list[on_proc_exit_index].arg);

```