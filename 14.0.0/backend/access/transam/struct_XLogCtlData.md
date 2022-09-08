#1.struct XLogCtlData

```cpp
/*
 * Total shared-memory state for XLOG.
 */
typedef struct XLogCtlData
{
	XLogCtlInsert Insert;

	/* Protected by info_lck: */
	XLogwrtRqst LogwrtRqst;
	XLogRecPtr	RedoRecPtr;		/* a recent copy of Insert->RedoRecPtr */
	FullTransactionId ckptFullXid;	/* nextXid of latest checkpoint */
	XLogRecPtr	asyncXactLSN;	/* LSN of newest async commit/abort */
	XLogRecPtr	replicationSlotMinLSN;	/* oldest LSN needed by any slot */

	XLogSegNo	lastRemovedSegNo;	/* latest removed/recycled XLOG segment */

	/* Fake LSN counter, for unlogged relations. Protected by ulsn_lck. */
	XLogRecPtr	unloggedLSN;
	slock_t		ulsn_lck;

	/* Time and LSN of last xlog segment switch. Protected by WALWriteLock. */
	pg_time_t	lastSegSwitchTime;
	XLogRecPtr	lastSegSwitchLSN;

	/*
	 * Protected by info_lck and WALWriteLock (you must hold either lock to
	 * read it, but both to update)
	 */
	XLogwrtResult LogwrtResult;

	/*
	 * Latest initialized page in the cache (last byte position + 1).
	 *
	 * To change the identity of a buffer (and InitializedUpTo), you need to
	 * hold WALBufMappingLock.  To change the identity of a buffer that's
	 * still dirty, the old page needs to be written out first, and for that
	 * you need WALWriteLock, and you need to ensure that there are no
	 * in-progress insertions to the page by calling
	 * WaitXLogInsertionsToFinish().
	 */
	XLogRecPtr	InitializedUpTo;

	/*
	 * These values do not change after startup, although the pointed-to pages
	 * and xlblocks values certainly do.  xlblocks values are protected by
	 * WALBufMappingLock.
	 */
	char	   *pages;			/* buffers for unwritten XLOG pages */
	XLogRecPtr *xlblocks;		/* 1st byte ptr-s + XLOG_BLCKSZ */
	int			XLogCacheBlck;	/* highest allocated xlog buffer index */

	/*
	 * Shared copy of ThisTimeLineID. Does not change after end-of-recovery.
	 * If we created a new timeline when the system was started up,
	 * PrevTimeLineID is the old timeline's ID that we forked off from.
	 * Otherwise it's equal to ThisTimeLineID.
	 */
	TimeLineID	ThisTimeLineID;
	TimeLineID	PrevTimeLineID;

	/*
	 * SharedRecoveryState indicates if we're still in crash or archive
	 * recovery.  Protected by info_lck.
	 */
	RecoveryState SharedRecoveryState;

	/*
	 * SharedHotStandbyActive indicates if we allow hot standby queries to be
	 * run.  Protected by info_lck.
	 */
	bool		SharedHotStandbyActive;

	/*
	 * SharedPromoteIsTriggered indicates if a standby promotion has been
	 * triggered.  Protected by info_lck.
	 */
	bool		SharedPromoteIsTriggered;

	/*
	 * WalWriterSleeping indicates whether the WAL writer is currently in
	 * low-power mode (and hence should be nudged if an async commit occurs).
	 * Protected by info_lck.
	 */
	bool		WalWriterSleeping;

	/*
	 * recoveryWakeupLatch is used to wake up the startup process to continue
	 * WAL replay, if it is waiting for WAL to arrive or failover trigger file
	 * to appear.
	 *
	 * Note that the startup process also uses another latch, its procLatch,
	 * to wait for recovery conflict. If we get rid of recoveryWakeupLatch for
	 * signaling the startup process in favor of using its procLatch, which
	 * comports better with possible generic signal handlers using that latch.
	 * But we should not do that because the startup process doesn't assume
	 * that it's waken up by walreceiver process or SIGHUP signal handler
	 * while it's waiting for recovery conflict. The separate latches,
	 * recoveryWakeupLatch and procLatch, should be used for inter-process
	 * communication for WAL replay and recovery conflict, respectively.
	 */
	Latch		recoveryWakeupLatch;

	/*
	 * During recovery, we keep a copy of the latest checkpoint record here.
	 * lastCheckPointRecPtr points to start of checkpoint record and
	 * lastCheckPointEndPtr points to end+1 of checkpoint record.  Used by the
	 * checkpointer when it wants to create a restartpoint.
	 *
	 * Protected by info_lck.
	 */
	XLogRecPtr	lastCheckPointRecPtr;
	XLogRecPtr	lastCheckPointEndPtr;
	CheckPoint	lastCheckPoint;

	/*
	 * lastReplayedEndRecPtr points to end+1 of the last record successfully
	 * replayed. When we're currently replaying a record, ie. in a redo
	 * function, replayEndRecPtr points to the end+1 of the record being
	 * replayed, otherwise it's equal to lastReplayedEndRecPtr.
	 */
	XLogRecPtr	lastReplayedEndRecPtr;
	TimeLineID	lastReplayedTLI;
	XLogRecPtr	replayEndRecPtr;
	TimeLineID	replayEndTLI;
	/* timestamp of last COMMIT/ABORT record replayed (or being replayed) */
	TimestampTz recoveryLastXTime;

	/*
	 * timestamp of when we started replaying the current chunk of WAL data,
	 * only relevant for replication or archive recovery
	 */
	TimestampTz currentChunkStartTime;
	/* Recovery pause state */
	RecoveryPauseState recoveryPauseState;
	ConditionVariable recoveryNotPausedCV;

	/*
	 * lastFpwDisableRecPtr points to the start of the last replayed
	 * XLOG_FPW_CHANGE record that instructs full_page_writes is disabled.
	 */
	XLogRecPtr	lastFpwDisableRecPtr;

	slock_t		info_lck;		/* locks shared variables shown above */
} XLogCtlData;

```