#1.struct XLogCtlInsert

```cpp
/*
 * Shared state data for WAL insertion.
 */
typedef struct XLogCtlInsert
{
	slock_t		insertpos_lck;	/* protects CurrBytePos and PrevBytePos */

	/*
	 * CurrBytePos is the end of reserved WAL. The next record will be
	 * inserted at that position. PrevBytePos is the start position of the
	 * previously inserted (or rather, reserved) record - it is copied to the
	 * prev-link of the next record. These are stored as "usable byte
	 * positions" rather than XLogRecPtrs (see XLogBytePosToRecPtr()).
	 */
	uint64		CurrBytePos;
	uint64		PrevBytePos;

	/*
	 * Make sure the above heavily-contended spinlock and byte positions are
	 * on their own cache line. In particular, the RedoRecPtr and full page
	 * write variables below should be on a different cache line. They are
	 * read on every WAL insertion, but updated rarely, and we don't want
	 * those reads to steal the cache line containing Curr/PrevBytePos.
	 */
	char		pad[PG_CACHE_LINE_SIZE];

	/*
	 * fullPageWrites is the authoritative value used by all backends to
	 * determine whether to write full-page image to WAL. This shared value,
	 * instead of the process-local fullPageWrites, is required because, when
	 * full_page_writes is changed by SIGHUP, we must WAL-log it before it
	 * actually affects WAL-logging by backends.  Checkpointer sets at startup
	 * or after SIGHUP.
	 *
	 * To read these fields, you must hold an insertion lock. To modify them,
	 * you must hold ALL the locks.
	 */
	XLogRecPtr	RedoRecPtr;		/* current redo point for insertions */
	bool		forcePageWrites;	/* forcing full-page writes for PITR? */
	bool		fullPageWrites;

	/*
	 * exclusiveBackupState indicates the state of an exclusive backup (see
	 * comments of ExclusiveBackupState for more details). nonExclusiveBackups
	 * is a counter indicating the number of streaming base backups currently
	 * in progress. forcePageWrites is set to true when either of these is
	 * non-zero. lastBackupStart is the latest checkpoint redo location used
	 * as a starting point for an online backup.
	 */
	ExclusiveBackupState exclusiveBackupState;
	int			nonExclusiveBackups;
	XLogRecPtr	lastBackupStart;

	/*
	 * WAL insertion locks.
	 */
	WALInsertLockPadded *WALInsertLocks;
} XLogCtlInsert;

```