#1.get lock

```cpp
1.A lock request is granted immediately if it does not conflict with
//1 新申请的锁，与已经授予的和处于等待状态的锁不冲突，则可授予
any existing or waiting lock request, or if the process already holds an
//2 本进程持有同样对象的锁，则可授予）
instance of the same lock type (eg, there's no penalty to acquire a read
lock twice).  Note that a process never conflicts with itself, eg one
//3 一个事务内的锁不互斥
can obtain read lock when one already holds exclusive lock.
2. Otherwise the process joins the lock's wait queue.  Normally it will
//4 上述加锁不成功，则进入锁等待队列的尾部
be added to the end of the queue, but there is an exception: if the
//5 一个优化点是：如果本进程已经在同一个对象上持有锁，
process already holds locks on this same lockable object that conflict
//  此对象上已经分配的锁与其他正处于等待的加锁请求冲突，那么：
with the request of any pending waiter, then the process will be
//  新申请的锁不进入锁等待队列的尾部，而是插入到正处于等待的、
inserted in the wait queue just ahead of the first such waiter.
(If we  //  与已经分配的锁冲突的加锁请求之前。
did not make this check, the deadlock detection code would adjust the
//  这个优化是死锁检测过程中完成的。
queue order to resolve the conflict, but it's relatively cheap to make
the check in ProcSleep and avoid a deadlock timeout delay in this case.)
Note special case when inserting before the end of the queue: if the
process's request does not conflict with any existing lock nor any
waiting request before its insertion point, then go ahead and grant the
lock without waiting.
```