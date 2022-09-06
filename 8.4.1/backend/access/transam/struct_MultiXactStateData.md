#1.struct MultiXactStateData

```cpp
/*
 * MultiXact state shared across all backends.  All this state is protected
 * by MultiXactGenLock.  (We also use MultiXactOffsetControlLock and
 * MultiXactMemberControlLock to guard accesses to the two sets of SLRU
 * buffers.  For concurrency's sake, we avoid holding more than one of these
 * locks at a time.)
 */
typedef struct MultiXactStateData
{
    /* next-to-be-assigned MultiXactId */
    MultiXactId nextMXact;

    /* next-to-be-assigned offset */
    MultiXactOffset nextOffset;

    /* the Offset SLRU area was last truncated at this MultiXactId */
    MultiXactId lastTruncationPoint;

    /*
     * Per-backend data starts here.  We have two arrays stored in the area
     * immediately following the MultiXactStateData struct. Each is indexed by
     * BackendId.
     *
     * In both arrays, there's a slot for all normal backends (1..MaxBackends)
     * followed by a slot for max_prepared_xacts prepared transactions. Valid
     * BackendIds start from 1; element zero of each array is never used.
     *
     * OldestMemberMXactId[k] is the oldest MultiXactId each backend's current
     * transaction(s) could possibly be a member of, or InvalidMultiXactId
     * when the backend has no live transaction that could possibly be a
     * member of a MultiXact.  Each backend sets its entry to the current
     * nextMXact counter just before first acquiring a shared lock in a given
     * transaction, and clears it at transaction end. (This works because only
     * during or after acquiring a shared lock could an XID possibly become a
     * member of a MultiXact, and that MultiXact would have to be created
     * during or after the lock acquisition.)
     *
     * OldestVisibleMXactId[k] is the oldest MultiXactId each backend's
     * current transaction(s) think is potentially live, or InvalidMultiXactId
     * when not in a transaction or not in a transaction that's paid any
     * attention to MultiXacts yet.  This is computed when first needed in a
     * given transaction, and cleared at transaction end.  We can compute it
     * as the minimum of the valid OldestMemberMXactId[] entries at the time
     * we compute it (using nextMXact if none are valid).  Each backend is
     * required not to attempt to access any SLRU data for MultiXactIds older
     * than its own OldestVisibleMXactId[] setting; this is necessary because
     * the checkpointer could truncate away such data at any instant.
     *
     * The checkpointer can compute the safe truncation point as the oldest
     * valid value among all the OldestMemberMXactId[] and
     * OldestVisibleMXactId[] entries, or nextMXact if none are valid.
     * Clearly, it is not possible for any later-computed OldestVisibleMXactId
     * value to be older than this, and so there is no risk of truncating data
     * that is still needed.
     */
    MultiXactId perBackendXactIds[1];   /* VARIABLE LENGTH ARRAY */
} MultiXactStateData;
 
```