#1.struct LOCALLOCK

```cpp
typedef struct LOCALLOCK
{
    LOCALLOCKTAG tag;            /* unique identifier of locallock entry */
    //加锁请求的标志
    /* data */
    LOCK       *lock;            /* associated LOCK object, if any */
    //加锁请求所申请到的锁
    PROCLOCK   *proclock;        /* associated PROCLOCK object, if any */
    //持锁者是哪个会话进程
    uint32     hashcode;         /* copy of LOCKTAG's hash value */
    //锁标志的hash值
    int64      nLocks;           /* total number of times lock is held */
    //锁被持有的次数
    int        numLockOwners;    /* # of relevant ResourceOwners */
    //与本锁对象相关的ResourceOwner有多少
    int        maxLockOwners;    /* allocated size of array */
    bool       holdsStrongLockCount;  /* bumped FastPathStrongRelationLocks */
    LOCALLOCKOWNER *lockOwners;       /* dynamically resizable array */
} LOCALLOCK;

```