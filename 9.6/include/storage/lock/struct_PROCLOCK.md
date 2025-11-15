#1.struct PROCLOCK

```cpp
/*
typedef struct PROCLOCKTAG
//标识会话进程和锁的关系，一个会话进程会持有很多锁，而一个锁只能被一个会话进程持有
{
    /* NB: we assume this struct contains no padding! */
    LOCK     *myLock;    /* link to per-lockable-object information */   //标识锁
    PGPROC    *myProc;    /* link to PGPROC of owning backend */ //标识会话进程
} PROCLOCKTAG;
typedef struct PROCLOCK      //标识会话进程和锁的关联关系
{
    /* tag */
    PROCLOCKTAG tag;  /* unique identifier of proclock object */  //标识会话进程与锁
    /* data */
    PGPROC      *groupLeader;    //为完成同一个任务而并行的进程属于同一个组，持锁被视为同
    一个事务
    LOCKMASK    holdMask;        /* bitmask for lock types currently held */
    //当前持有锁者持有的锁类型
    LOCKMASK    releaseMask;     /* bitmask for lock types to be released */
    //当前持有锁者释放的锁类型
    SHM_QUEUE   lockLink;        /* list link in LOCK's list of proclocks */
    //本对象PROCLOCKTAG在锁的链表中的位置，参见“锁对象”一节
    SHM_QUEUE   procLink;        /* list link in PGPROC's list of proclocks */
   //本对象PROCLOCKTAG在进程链表中的位置
} PROCLOCK;


 * We may have several different backends holding or awaiting locks
 * on the same lockable object.  We need to store some per-holder/waiter
 * information for each such holder (or would-be holder).  This is kept in
 * a PROCLOCK struct.
 *
 * PROCLOCKTAG is the key information needed to look up a PROCLOCK item in the
 * proclock hashtable.  A PROCLOCKTAG value uniquely identifies the combination
 * of a lockable object and a holder/waiter for that object.  (We can use
 * pointers here because the PROCLOCKTAG need only be unique for the lifespan
 * of the PROCLOCK, and it will never outlive the lock or the proc.)
 *
 * Internally to a backend, it is possible for the same lock to be held
 * for different purposes: the backend tracks transaction locks separately
 * from session locks.  However, this is not reflected in the shared-memory
 * state: we only track which backend(s) hold the lock.  This is OK since a
 * backend can never block itself.
 *
 * The holdMask field shows the already-granted locks represented by this
 * proclock.  Note that there will be a proclock object, possibly with
 * zero holdMask, for any lock that the process is currently waiting on.
 * Otherwise, proclock objects whose holdMasks are zero are recycled
 * as soon as convenient.
 *
 * releaseMask is workspace for LockReleaseAll(): it shows the locks due
 * to be released during the current call.  This must only be examined or
 * set by the backend owning the PROCLOCK.
 *
 * Each PROCLOCK object is linked into lists for both the associated LOCK
 * object and the owning PGPROC object.  Note that the PROCLOCK is entered
 * into these lists as soon as it is created, even if no lock has yet been
 * granted.  A PGPROC that is waiting for a lock to be granted will also be
 * linked into the lock's waitProcs queue.
 */
typedef struct PROCLOCKTAG
{
	/* NB: we assume this struct contains no padding! */
	LOCK	   *myLock;			/* link to per-lockable-object information */
	PGPROC	   *myProc;			/* link to PGPROC of owning backend */
} PROCLOCKTAG;

typedef struct PROCLOCK
{
	/* tag */
	PROCLOCKTAG tag;			/* unique identifier of proclock object */

	/* data */
	PGPROC	   *groupLeader;	/* proc's lock group leader, or proc itself */
	LOCKMASK	holdMask;		/* bitmask for lock types currently held */
	LOCKMASK	releaseMask;	/* bitmask for lock types to be released */
	SHM_QUEUE	lockLink;		/* list link in LOCK's list of proclocks */
	SHM_QUEUE	procLink;		/* list link in PGPROC's list of proclocks */
} PROCLOCK;


```