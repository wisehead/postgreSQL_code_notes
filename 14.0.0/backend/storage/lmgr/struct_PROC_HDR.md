#1.struct PROC_HDR

```cpp
/*
 * There is one ProcGlobal struct for the whole database cluster.
 *
 * Adding/Removing an entry into the procarray requires holding *both*
 * ProcArrayLock and XidGenLock in exclusive mode (in that order). Both are
 * needed because the dense arrays (see below) are accessed from
 * GetNewTransactionId() and GetSnapshotData(), and we don't want to add
 * further contention by both using the same lock. Adding/Removing a procarray
 * entry is much less frequent.
 *
 * Some fields in PGPROC are mirrored into more densely packed arrays (e.g.
 * xids), with one entry for each backend. These arrays only contain entries
 * for PGPROCs that have been added to the shared array with ProcArrayAdd()
 * (in contrast to PGPROC array which has unused PGPROCs interspersed).
 *
 * The dense arrays are indexed by PGPROC->pgxactoff. Any concurrent
 * ProcArrayAdd() / ProcArrayRemove() can lead to pgxactoff of a procarray
 * member to change.  Therefore it is only safe to use PGPROC->pgxactoff to
 * access the dense array while holding either ProcArrayLock or XidGenLock.
 *
 * As long as a PGPROC is in the procarray, the mirrored values need to be
 * maintained in both places in a coherent manner.
 *
 * The denser separate arrays are beneficial for three main reasons: First, to
 * allow for as tight loops accessing the data as possible. Second, to prevent
 * updates of frequently changing data (e.g. xmin) from invalidating
 * cachelines also containing less frequently changing data (e.g. xid,
 * statusFlags). Third to condense frequently accessed data into as few
 * cachelines as possible.
 *
 * There are two main reasons to have the data mirrored between these dense
 * arrays and PGPROC. First, as explained above, a PGPROC's array entries can
 * only be accessed with either ProcArrayLock or XidGenLock held, whereas the
 * PGPROC entries do not require that (obviously there may still be locking
 * requirements around the individual field, separate from the concerns
 * here). That is particularly important for a backend to efficiently checks
 * it own values, which it often can safely do without locking.  Second, the
 * PGPROC fields allow to avoid unnecessary accesses and modification to the
 * dense arrays. A backend's own PGPROC is more likely to be in a local cache,
 * whereas the cachelines for the dense array will be modified by other
 * backends (often removing it from the cache for other cores/sockets). At
 * commit/abort time a check of the PGPROC value can avoid accessing/dirtying
 * the corresponding array value.
 *
 * Basically it makes sense to access the PGPROC variable when checking a
 * single backend's data, especially when already looking at the PGPROC for
 * other reasons already.  It makes sense to look at the "dense" arrays if we
 * need to look at many / most entries, because we then benefit from the
 * reduced indirection and better cross-process cache-ability.
 *
 * When entering a PGPROC for 2PC transactions with ProcArrayAdd(), the data
 * in the dense arrays is initialized from the PGPROC while it already holds
 * ProcArrayLock.
 */
typedef struct PROC_HDR
{
	/* Array of PGPROC structures (not including dummies for prepared txns) */
	PGPROC	   *allProcs;

	/* Array mirroring PGPROC.xid for each PGPROC currently in the procarray */
	TransactionId *xids;

	/*
	 * Array mirroring PGPROC.subxidStatus for each PGPROC currently in the
	 * procarray.
	 */
	XidCacheStatus *subxidStates;

	/*
	 * Array mirroring PGPROC.statusFlags for each PGPROC currently in the
	 * procarray.
	 */
	uint8	   *statusFlags;

	/* Length of allProcs array */
	uint32		allProcCount;
	/* Head of list of free PGPROC structures */
	PGPROC	   *freeProcs;
	/* Head of list of autovacuum's free PGPROC structures */
	PGPROC	   *autovacFreeProcs;
	/* Head of list of bgworker free PGPROC structures */
	PGPROC	   *bgworkerFreeProcs;
	/* Head of list of walsender free PGPROC structures */
	PGPROC	   *walsenderFreeProcs;
	/* First pgproc waiting for group XID clear */
	pg_atomic_uint32 procArrayGroupFirst;
	/* First pgproc waiting for group transaction status update */
	pg_atomic_uint32 clogGroupFirst;
	/* WALWriter process's latch */
	Latch	   *walwriterLatch;
	/* Checkpointer process's latch */
	Latch	   *checkpointerLatch;
	/* Current shared estimate of appropriate spins_per_delay value */
	int			spins_per_delay;
	/* The proc of the Startup process, since not in ProcArray */
	PGPROC	   *startupProc;
	int			startupProcPid;
	/* Buffer id of the buffer that Startup process waits for pin on, or -1 */
	int			startupBufferPinWaitBufId;
} PROC_HDR;


```