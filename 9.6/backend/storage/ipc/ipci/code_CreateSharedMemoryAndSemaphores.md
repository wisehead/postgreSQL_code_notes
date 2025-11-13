#1.CreateSharedMemoryAndSemaphores

```cpp
/*
 * CreateSharedMemoryAndSemaphores
 *		Creates and initializes shared memory and semaphores.
 *
 * This is called by the postmaster or by a standalone backend.
 * It is also called by a backend forked from the postmaster in the
 * EXEC_BACKEND case.  In the latter case, the shared memory segment
 * already exists and has been physically attached to, but we have to
 * initialize pointers in local memory that reference the shared structures,
 * because we didn't inherit the correct pointer values from the postmaster
 * as we do in the fork() scenario.  The easiest way to do that is to run
 * through the same code as before.  (Note that the called routines mostly
 * check IsUnderPostmaster, rather than EXEC_BACKEND, to detect this case.
 * This is a bit code-wasteful and could be cleaned up.)
 */
 void
CreateSharedMemoryAndSemaphores(bool makePrivate, int port)  //创建共享内存
{...
    if (!IsUnderPostmaster) //系统处于启动状态
    {
...
        size = 100000;
        size = add_size(size, SpinlockSemaSize());
        //SpinLock信号量将被初始化，用于只能使用semaphore模拟SpinLock的情况
...
        size = add_size(size, LockShmemSize());
        //计算用于锁共享缓存的锁表的大小，包括：lock hash table、proclock hash table
        size = add_size(size, PredicateLockShmemSize());
        //计算用于谓词锁表的大小，包括：predicate lock hash table、rw-conflict pool等
...
        size = add_size(size, LWLockShmemSize()); //计算LWLock锁表的大小
...
    }
    else
    {...}
...
    // Now initialize LWLocks, which do shared memory allocation and are needed
       for InitShmemIndex.
    CreateLWLocks(); //创建LWLock，即初始化LWLock，包括“MainLWLockArray”
...
    InitLocks(); //初始化锁管理器，包括：LockMethodLockHash（位于共享内存）、
    LockMethodProcLockHash（位于共享内存）、LockMethodLocalHash（位于进程的局部内存）
    InitPredicateLocks();    //初始化用于实现SSI技术的谓词锁表
...
    CheckpointerShmemInit(); //CheckPoint初始化
...
}

```