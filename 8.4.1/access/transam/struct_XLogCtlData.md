#1.struct XLogCtlData

```cpp
/*
 * Total shared-memory state for XLOG.
 */
typedef struct XLogCtlData
{
    /* Protected by WALInsertLock: */
    XLogCtlInsert Insert;

    /* Protected by info_lck: */
    XLogwrtRqst LogwrtRqst;
    XLogwrtResult LogwrtResult;
    uint32      ckptXidEpoch;   /* nextXID & epoch of latest checkpoint */
    TransactionId ckptXid;
    XLogRecPtr  asyncCommitLSN; /* LSN of newest async commit */

    /* Protected by WALWriteLock: */
    XLogCtlWrite Write;

    /*
     * These values do not change after startup, although the pointed-to pages
     * and xlblocks values certainly do.  Permission to read/write the pages
     * and xlblocks values depends on WALInsertLock and WALWriteLock.
     */
    char       *pages;          /* buffers for unwritten XLOG pages */
    XLogRecPtr *xlblocks;       /* 1st byte ptr-s + XLOG_BLCKSZ */
    int         XLogCacheBlck;  /* highest allocated xlog buffer index */
    TimeLineID  ThisTimeLineID;

    /*
     * SharedRecoveryInProgress indicates if we're still in crash or archive
     * recovery.  Protected by info_lck.
     */
    bool        SharedRecoveryInProgress;

    /*
     * During recovery, we keep a copy of the latest checkpoint record here.
     * Used by the background writer when it wants to create a restartpoint.
     *
     * Protected by info_lck.
     */
    XLogRecPtr  lastCheckPointRecPtr;
    CheckPoint  lastCheckPoint;

    /* end+1 of the last record replayed (or being replayed) */
    XLogRecPtr  replayEndRecPtr;

    slock_t     info_lck;       /* locks shared variables shown above */
} XLogCtlData;
```