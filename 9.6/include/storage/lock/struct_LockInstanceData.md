#1.struct LockInstanceData

```cpp
typedef struct LockInstanceData
{
    LOCKTAG    locktag;       /* 被加锁的对象的标识 */
    LOCKMASK    holdMask;      /* locks held by this PGPROC，值为
    “LOCKBIT_ON(ExclusiveLock)”表示被加锁 */
    LOCKMODE   waitLockMode;  /* lock awaited by this PGPROC, if any */
    BackendId  backend;       /* backend ID of this PGPROC */
    LocalTransactionId lxid;  /* local transaction ID of this PGPROC */
    int        pid;           /* pid of this PGPROC */
    int        leaderPid;     /* pid of group leader; = pid if no group */
    bool       fastpath;      /* taken via fastpath? */
} LockInstanceData;

```