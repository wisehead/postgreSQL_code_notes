#1.LockConflicts

```cpp
static const LOCKMASK LockConflicts[] = {
     0, //值为零，表示空一行，下一行才是真的锁模式
    //检查与AccessExclusiveLock锁的相容性
     LOCKBIT_ON(AccessExclusiveLock), //对于AccessShareLock类型的持锁，2左移8位是258，
     其二级制为“100000000”，后八位是零，对于8种可能申请的锁类型，与零匹配之后还是零，表示不
     相容。如下依次计算方式类推，可以知道新申请的锁与持有的锁是否相容
    //检查与RowShareLock锁的相容性
    LOCKBIT_ON(ExclusiveLock) | LOCKBIT_ON(AccessExclusiveLock),
    //检查与RowShareLock锁的相容性
    //检查与RowExclusiveLock锁的相容性
    LOCKBIT_ON(ShareLock) | LOCKBIT_ON(ShareRowExclusiveLock) | LOCKBIT_
ON(ExclusiveLock) | LOCKBIT_ON(AccessExclusiveLock),
    //检查与ShareUpdateExclusiveLock锁的相容性
    LOCKBIT_ON(ShareUpdateExclusiveLock) |
    LOCKBIT_ON(ShareLock) | LOCKBIT_ON(ShareRowExclusiveLock) | LOCKBIT_
    ON(ExclusiveLock) | LOCKBIT_ON(AccessExclusiveLock),
    //检查与ShareLock锁的相容性
    LOCKBIT_ON(RowExclusiveLock) | LOCKBIT_ON(ShareUpdateExclusiveLock) |
    LOCKBIT_ON(ShareRowExclusiveLock) | LOCKBIT_ON(ExclusiveLock) | LOCKBIT_
    ON(AccessExclusiveLock),
    //检查与ShareRowExclusiveLock锁的相容性
    LOCKBIT_ON(RowExclusiveLock) | LOCKBIT_ON(ShareUpdateExclusiveLock) |
    LOCKBIT_ON(ShareLock) | LOCKBIT_ON(ShareRowExclusiveLock) | LOCKBIT_
    ON(ExclusiveLock) | LOCKBIT_ON(AccessExclusiveLock),
    //检查与ExclusiveLock锁的相容性
    LOCKBIT_ON(RowShareLock) | LOCKBIT_ON(RowExclusiveLock) | LOCKBIT_
    ON(ShareUpdateExclusiveLock) |
    LOCKBIT_ON(ShareLock) | LOCKBIT_ON(ShareRowExclusiveLock) | LOCKBIT_
    ON(ExclusiveLock) | LOCKBIT_ON(AccessExclusiveLock),
    //检查与AccessExclusiveLock锁的相容性
    LOCKBIT_ON(AccessShareLock) | LOCKBIT_ON(RowShareLock) |  LOCKBIT_
    ON(RowExclusiveLock) | LOCKBIT_ON(ShareUpdateExclusiveLock) |
    LOCKBIT_ON(ShareLock) | LOCKBIT_ON(ShareRowExclusiveLock) | LOCKBIT_
    ON(ExclusiveLock) | LOCKBIT_ON(AccessExclusiveLock)
};

```