#1.struct PGPROC

```cpp
/*
 * Each backend has a PGPROC struct in shared memory.  There is also a list of
 * currently-unused PGPROC structs that will be reallocated to new backends.
 *
 * links: list link for any list the PGPROC is in.  When waiting for a lock,
 * the PGPROC is linked into that lock's waitProcs queue.  A recycled PGPROC
 * is linked into ProcGlobal's freeProcs list.
 *
 * Note: twophase.c also sets up a dummy PGPROC struct for each currently
 * prepared transaction.  These PGPROCs appear in the ProcArray data structure
 * so that the prepared transactions appear to be still running and are
 * correctly shown as holding locks.  A prepared transaction PGPROC can be
 * distinguished from a real one at need by the fact that it has pid == 0.
 * The semaphore and lock-activity fields in a prepared-xact PGPROC are unused,
 * but its myProcLocks[] lists are valid.
 *
 * We allow many fields of this struct to be accessed without locks, such as
 * delayChkpt and isBackgroundWorker. However, keep in mind that writing
 * mirrored ones (see below) requires holding ProcArrayLock or XidGenLock in
 * at least shared mode, so that pgxactoff does not change concurrently.
 *
 * Mirrored fields:
 *
 * Some fields in PGPROC (see "mirrored in ..." comment) are mirrored into an
 * element of more densely packed ProcGlobal arrays. These arrays are indexed
 * by PGPROC->pgxactoff. Both copies need to be maintained coherently.
 *
 * NB: The pgxactoff indexed value can *never* be accessed without holding
 * locks.
 *
 * See PROC_HDR for details.
 */
struct PGPROC
{
	/* proc->links MUST BE FIRST IN STRUCT (see ProcSleep,ProcWakeup,etc) */
	SHM_QUEUE	links;			/* list link if process is in a list */
	PGPROC	  **procgloballist; /* procglobal list that owns this PGPROC */

	PGSemaphore sem;			/* ONE semaphore to sleep on */
	ProcWaitStatus waitStatus;

	Latch		procLatch;		/* generic latch for process */


	TransactionId xid;			/* id of top-level transaction currently being
								 * executed by this proc, if running and XID
								 * is assigned; else InvalidTransactionId.
								 * mirrored in ProcGlobal->xids[pgxactoff] */

	TransactionId xmin;			/* minimal running XID as it was when we were
								 * starting our xact, excluding LAZY VACUUM:
								 * vacuum must not remove tuples deleted by
								 * xid >= xmin ! */

	LocalTransactionId lxid;	/* local id of top-level transaction currently
								 * being executed by this proc, if running;
								 * else InvalidLocalTransactionId */
	int			pid;			/* Backend's process ID; 0 if prepared xact */

	int			pgxactoff;		/* offset into various ProcGlobal->arrays with
								 * data mirrored from this PGPROC */
	int			pgprocno;

	/* These fields are zero while a backend is still starting up: */
	BackendId	backendId;		/* This backend's backend ID (if assigned) */
	Oid			databaseId;		/* OID of database this backend is using */
	Oid			roleId;			/* OID of role using this backend */

	Oid			tempNamespaceId;	/* OID of temp schema this backend is
									 * using */

	bool		isBackgroundWorker; /* true if background worker. */

	/*
	 * While in hot standby mode, shows that a conflict signal has been sent
	 * for the current transaction. Set/cleared while holding ProcArrayLock,
	 * though not required. Accessed without lock, if needed.
	 */
	bool		recoveryConflictPending;

	/* Info about LWLock the process is currently waiting for, if any. */
	bool		lwWaiting;		/* true if waiting for an LW lock */
	uint8		lwWaitMode;		/* lwlock mode being waited for */
	proclist_node lwWaitLink;	/* position in LW lock wait list */

	/* Support for condition variables. */
	proclist_node cvWaitLink;	/* position in CV wait list */

	/* Info about lock the process is currently waiting for, if any. */
	/* waitLock and waitProcLock are NULL if not currently waiting. */
	LOCK	   *waitLock;		/* Lock object we're sleeping on ... */
	PROCLOCK   *waitProcLock;	/* Per-holder info for awaited lock */
	LOCKMODE	waitLockMode;	/* type of lock we're waiting for */
	LOCKMASK	heldLocks;		/* bitmask for lock types already held on this
								 * lock object by this backend */
	pg_atomic_uint64 waitStart; /* time at which wait for lock acquisition
								 * started */

	bool		delayChkpt;		/* true if this proc delays checkpoint start */

	uint8		statusFlags;	/* this backend's status flags, see PROC_*
								 * above. mirrored in
								 * ProcGlobal->statusFlags[pgxactoff] */

	/*
	 * Info to allow us to wait for synchronous replication, if needed.
	 * waitLSN is InvalidXLogRecPtr if not waiting; set only by user backend.
	 * syncRepState must not be touched except by owning process or WALSender.
	 * syncRepLinks used only while holding SyncRepLock.
	 */
	XLogRecPtr	waitLSN;		/* waiting for this LSN or higher */
	int			syncRepState;	/* wait state for sync rep */
	SHM_QUEUE	syncRepLinks;	/* list link if process is in syncrep queue */

	/*
	 * All PROCLOCK objects for locks held or awaited by this backend are
	 * linked into one of these lists, according to the partition number of
	 * their lock.
	 */
	SHM_QUEUE	myProcLocks[NUM_LOCK_PARTITIONS];

	XidCacheStatus subxidStatus;	/* mirrored with
									 * ProcGlobal->subxidStates[i] */
	struct XidCache subxids;	/* cache for subtransaction XIDs */

	/* Support for group XID clearing. */
	/* true, if member of ProcArray group waiting for XID clear */
	bool		procArrayGroupMember;
	/* next ProcArray group member waiting for XID clear */
	pg_atomic_uint32 procArrayGroupNext;

	/*
	 * latest transaction id among the transaction's main XID and
	 * subtransactions
	 */
	TransactionId procArrayGroupMemberXid;

	uint32		wait_event_info;	/* proc's wait information */

	/* Support for group transaction status update. */
	bool		clogGroupMember;	/* true, if member of clog group */
	pg_atomic_uint32 clogGroupNext; /* next clog group member */
	TransactionId clogGroupMemberXid;	/* transaction id of clog group member */
	XidStatus	clogGroupMemberXidStatus;	/* transaction status of clog
											 * group member */
	int			clogGroupMemberPage;	/* clog page corresponding to
										 * transaction id of clog group member */
	XLogRecPtr	clogGroupMemberLsn; /* WAL location of commit record for clog
									 * group member */

	/* Lock manager data, recording fast-path locks taken by this backend. */
	LWLock		fpInfoLock;		/* protects per-backend fast-path state */
	uint64		fpLockBits;		/* lock modes held for each fast-path slot */
	Oid			fpRelId[FP_LOCK_SLOTS_PER_BACKEND]; /* slots for rel oids */
	bool		fpVXIDLock;		/* are we holding a fast-path VXID lock? */
	LocalTransactionId fpLocalTransactionId;	/* lxid for fast-path VXID
												 * lock */

	/*
	 * Support for lock groups.  Use LockHashPartitionLockByProc on the group
	 * leader to get the LWLock protecting these fields.
	 */
	PGPROC	   *lockGroupLeader;	/* lock group leader, if I'm a member */
	dlist_head	lockGroupMembers;	/* list of members, if I'm a leader */
	dlist_node	lockGroupLink;	/* my member link, if I'm a member */
};

```