#1.CreateLWLocks

```cpp
//Allocate shmem space for the main LWLock array and all tranches and initialize
 it.  We also register all the LWLock tranches here.
void
CreateLWLocks(void)  //在共享内存中创建所有的系统级LWLock
{...
    if (!IsUnderPostmaster)
    {
        Size spaceLocks = LWLockShmemSize(); //可以求知LWLock锁需要的空间大小，从中
可以分析出有多少种系统级LWLock，详情如下
...
        ptr = (char *) ShmemAlloc(spaceLocks); //在共享内存中分配空间
...
        MainLWLockArray = (LWLockPadded *) ptr; //记录地址
...
        /* Initialize all LWLocks */
        InitializeLWLocks(); //在共享内存中分配了空间给系统级的LWLock后，对他们进行初始化
    }
    /* Register all LWLock tranches */
    RegisterLWLockTranches();  //注册即加载已经所有的系统级的LWLock（如"main"、
"buffer_mapping"、"lock_manager"、"predicate_lock_manager"）
}
```