#1.heap_delete

```cpp
/*
 *  heap_delete - delete a tuple
 *
 * NB: do not call this directly unless you are prepared to deal with
 * concurrent-update conditions.  Use simple_heap_delete instead.
 *
 *  relation - table to be modified (caller must hold suitable lock)
 *  tid - TID of tuple to be deleted
 *  ctid - output parameter, used only for failure case (see below)
 *  update_xmax - output parameter, used only for failure case (see below)
 *  cid - delete command ID (used for visibility test, and stored into
 *      cmax if successful)
 *  crosscheck - if not InvalidSnapshot, also check tuple against this
 *  wait - true if should wait for any conflicting update to commit/abort
 *
 * Normal, successful return value is HeapTupleMayBeUpdated, which
 * actually means we did delete it.  Failure return codes are
 * HeapTupleSelfUpdated, HeapTupleUpdated, or HeapTupleBeingUpdated
 * (the last only possible if wait == false).
 *
 * In the failure cases, the routine returns the tuple's t_ctid and t_xmax.
 * If t_ctid is the same as tid, the tuple was deleted; if different, the
 * tuple was updated, and t_ctid is the location of the replacement tuple.
 * (t_xmax is needed to verify that the replacement tuple matches.)
 */
HTSU_Result
heap_delete(Relation relation, ItemPointer tid,
            ItemPointer ctid, TransactionId *update_xmax,
            CommandId cid, Snapshot crosscheck, bool wait)

```