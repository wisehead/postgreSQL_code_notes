#1.struct TableAmRoutine

```cpp
/*
 * API struct for a table AM.  Note this must be allocated in a
 * server-lifetime manner, typically as a static const struct, which then gets
 * returned by FormData_pg_am.amhandler.
 *
 * In most cases it's not appropriate to call the callbacks directly, use the
 * table_* wrapper functions instead.
 *
 * GetTableAmRoutine() asserts that required callbacks are filled in, remember
 * to update when adding a callback.
 */
 typedef struct TableAmRoutine
{
	/* this must be set to T_TableAmRoutine */
	NodeTag		type;


	/* ------------------------------------------------------------------------
	 * Slot related callbacks.
	 * ------------------------------------------------------------------------
	 */

	/*
	 * Return slot implementation suitable for storing a tuple of this AM.
	 */
	const TupleTableSlotOps *(*slot_callbacks) (Relation rel);


	/* ------------------------------------------------------------------------
	 * Table scan callbacks.
	 * ------------------------------------------------------------------------
	 */

	/*
	 * Start a scan of `rel`.  The callback has to return a TableScanDesc,
	 * which will typically be embedded in a larger, AM specific, struct.
	 *
	 * If nkeys != 0, the results need to be filtered by those scan keys.
	 *
	 * pscan, if not NULL, will have already been initialized with
	 * parallelscan_initialize(), and has to be for the same relation. Will
	 * only be set coming from table_beginscan_parallel().
	 *
	 * `flags` is a bitmask indicating the type of scan (ScanOptions's
	 * SO_TYPE_*, currently only one may be specified), options controlling
	 * the scan's behaviour (ScanOptions's SO_ALLOW_*, several may be
	 * specified, an AM may ignore unsupported ones) and whether the snapshot
	 * needs to be deallocated at scan_end (ScanOptions's SO_TEMP_SNAPSHOT).
	 */
	TableScanDesc (*scan_begin) (Relation rel,
								 Snapshot snapshot,
								 int nkeys, struct ScanKeyData *key,
								 ParallelTableScanDesc pscan,
								 uint32 flags);

	/*
	 * Release resources and deallocate scan. If TableScanDesc.temp_snap,
	 * TableScanDesc.rs_snapshot needs to be unregistered.
	 */
	void		(*scan_end) (TableScanDesc scan);

	/*
	 * Restart relation scan.  If set_params is set to true, allow_{strat,
	 * sync, pagemode} (see scan_begin) changes should be taken into account.
	 */
	void		(*scan_rescan) (TableScanDesc scan, struct ScanKeyData *key,
								bool set_params, bool allow_strat,
								bool allow_sync, bool allow_pagemode);

	/*
	 * Return next tuple from `scan`, store in slot.
	 */
	bool		(*scan_getnextslot) (TableScanDesc scan,
									 ScanDirection direction,
									 TupleTableSlot *slot);

	/*-----------
	 * Optional functions to provide scanning for ranges of ItemPointers.
	 * Implementations must either provide both of these functions, or neither
	 * of them.
	 *
	 * Implementations of scan_set_tidrange must themselves handle
	 * ItemPointers of any value. i.e, they must handle each of the following:
	 *
	 * 1) mintid or maxtid is beyond the end of the table; and
	 * 2) mintid is above maxtid; and
	 * 3) item offset for mintid or maxtid is beyond the maximum offset
	 * allowed by the AM.
	 *
	 * Implementations can assume that scan_set_tidrange is always called
	 * before can_getnextslot_tidrange or after scan_rescan and before any
	 * further calls to scan_getnextslot_tidrange.
	 */
	void		(*scan_set_tidrange) (TableScanDesc scan,
									  ItemPointer mintid,
									  ItemPointer maxtid);

	/*
	 * Return next tuple from `scan` that's in the range of TIDs defined by
	 * scan_set_tidrange.
	 */
	bool		(*scan_getnextslot_tidrange) (TableScanDesc scan,
											  ScanDirection direction,
											  TupleTableSlot *slot);

	/* ------------------------------------------------------------------------
	 * Parallel table scan related functions.
	 * ------------------------------------------------------------------------
	 */

	/*
	 * Estimate the size of shared memory needed for a parallel scan of this
	 * relation. The snapshot does not need to be accounted for.
	 */
	Size		(*parallelscan_estimate) (Relation rel);

	/*
	 * Initialize ParallelTableScanDesc for a parallel scan of this relation.
	 * `pscan` will be sized according to parallelscan_estimate() for the same
	 * relation.
	 */
	Size		(*parallelscan_initialize) (Relation rel,
											ParallelTableScanDesc pscan);

	/*
	 * Reinitialize `pscan` for a new scan. `rel` will be the same relation as
	 * when `pscan` was initialized by parallelscan_initialize.
	 */
	void		(*parallelscan_reinitialize) (Relation rel,
											  ParallelTableScanDesc pscan);


	/* ------------------------------------------------------------------------
	 * Index Scan Callbacks
	 * ------------------------------------------------------------------------
	 */

	/*
	 * Prepare to fetch tuples from the relation, as needed when fetching
	 * tuples for an index scan.  The callback has to return an
	 * IndexFetchTableData, which the AM will typically embed in a larger
	 * structure with additional information.
	 *
	 * Tuples for an index scan can then be fetched via index_fetch_tuple.
	 */
	struct IndexFetchTableData *(*index_fetch_begin) (Relation rel);

	/*
	 * Reset index fetch. Typically this will release cross index fetch
	 * resources held in IndexFetchTableData.
	 */
	void		(*index_fetch_reset) (struct IndexFetchTableData *data);

	/*
	 * Release resources and deallocate index fetch.
	 */
	void		(*index_fetch_end) (struct IndexFetchTableData *data);

	/*
	 * Fetch tuple at `tid` into `slot`, after doing a visibility test
	 * according to `snapshot`. If a tuple was found and passed the visibility
	 * test, return true, false otherwise.
	 *
	 * Note that AMs that do not necessarily update indexes when indexed
	 * columns do not change, need to return the current/correct version of
	 * the tuple that is visible to the snapshot, even if the tid points to an
	 * older version of the tuple.
	 *
	 * *call_again is false on the first call to index_fetch_tuple for a tid.
	 * If there potentially is another tuple matching the tid, *call_again
	 * needs to be set to true by index_fetch_tuple, signaling to the caller
	 * that index_fetch_tuple should be called again for the same tid.
	 *
	 * *all_dead, if all_dead is not NULL, should be set to true by
	 * index_fetch_tuple iff it is guaranteed that no backend needs to see
	 * that tuple. Index AMs can use that to avoid returning that tid in
	 * future searches.
	 */
	bool		(*index_fetch_tuple) (struct IndexFetchTableData *scan,
									  ItemPointer tid,
									  Snapshot snapshot,
									  TupleTableSlot *slot,
									  bool *call_again, bool *all_dead);


	/* ------------------------------------------------------------------------
	 * Callbacks for non-modifying operations on individual tuples
	 * ------------------------------------------------------------------------
	 */

	/*
	 * Fetch tuple at `tid` into `slot`, after doing a visibility test
	 * according to `snapshot`. If a tuple was found and passed the visibility
	 * test, returns true, false otherwise.
	 */
	bool		(*tuple_fetch_row_version) (Relation rel,
											ItemPointer tid,
											Snapshot snapshot,
											TupleTableSlot *slot);

	/*
	 * Is tid valid for a scan of this relation.
	 */
	bool		(*tuple_tid_valid) (TableScanDesc scan,
									ItemPointer tid);

	/*
	 * Return the latest version of the tuple at `tid`, by updating `tid` to
	 * point at the newest version.
	 */
	void		(*tuple_get_latest_tid) (TableScanDesc scan,
										 ItemPointer tid);

	/*
	 * Does the tuple in `slot` satisfy `snapshot`?  The slot needs to be of
	 * the appropriate type for the AM.
	 */
	bool		(*tuple_satisfies_snapshot) (Relation rel,
											 TupleTableSlot *slot,
											 Snapshot snapshot);

	/* see table_index_delete_tuples() */
	TransactionId (*index_delete_tuples) (Relation rel,
										  TM_IndexDeleteOp *delstate);


	/* ------------------------------------------------------------------------
	 * Manipulations of physical tuples.
	 * ------------------------------------------------------------------------
	 */

	/* see table_tuple_insert() for reference about parameters */
	void		(*tuple_insert) (Relation rel, TupleTableSlot *slot,
								 CommandId cid, int options,
								 struct BulkInsertStateData *bistate);

	/* see table_tuple_insert_speculative() for reference about parameters */
	void		(*tuple_insert_speculative) (Relation rel,
											 TupleTableSlot *slot,
											 CommandId cid,
											 int options,
											 struct BulkInsertStateData *bistate,
											 uint32 specToken);

	/* see table_tuple_complete_speculative() for reference about parameters */
	void		(*tuple_complete_speculative) (Relation rel,
											   TupleTableSlot *slot,
											   uint32 specToken,
											   bool succeeded);

	/* see table_multi_insert() for reference about parameters */
	void		(*multi_insert) (Relation rel, TupleTableSlot **slots, int nslots,
								 CommandId cid, int options, struct BulkInsertStateData *bistate);

	/* see table_tuple_delete() for reference about parameters */
	TM_Result	(*tuple_delete) (Relation rel,
								 ItemPointer tid,
								 CommandId cid,
								 Snapshot snapshot,
								 Snapshot crosscheck,
								 bool wait,
								 TM_FailureData *tmfd,
								 bool changingPart);

	/* see table_tuple_update() for reference about parameters */
	TM_Result	(*tuple_update) (Relation rel,
								 ItemPointer otid,
								 TupleTableSlot *slot,
								 CommandId cid,
								 Snapshot snapshot,
								 Snapshot crosscheck,
								 bool wait,
								 TM_FailureData *tmfd,
								 LockTupleMode *lockmode,
								 bool *update_indexes);

	/* see table_tuple_lock() for reference about parameters */
	TM_Result	(*tuple_lock) (Relation rel,
							   ItemPointer tid,
							   Snapshot snapshot,
							   TupleTableSlot *slot,
							   CommandId cid,
							   LockTupleMode mode,
							   LockWaitPolicy wait_policy,
							   uint8 flags,
							   TM_FailureData *tmfd);

	/*
	 * Perform operations necessary to complete insertions made via
	 * tuple_insert and multi_insert with a BulkInsertState specified. In-tree
	 * access methods ceased to use this.
	 *
	 * Typically callers of tuple_insert and multi_insert will just pass all
	 * the flags that apply to them, and each AM has to decide which of them
	 * make sense for it, and then only take actions in finish_bulk_insert for
	 * those flags, and ignore others.
	 *
	 * Optional callback.
	 */
	void		(*finish_bulk_insert) (Relation rel, int options);


	/* ------------------------------------------------------------------------
	 * DDL related functionality.
	 * ------------------------------------------------------------------------
	 */

	/*
	 * This callback needs to create a new relation filenode for `rel`, with
	 * appropriate durability behaviour for `persistence`.
	 *
	 * Note that only the subset of the relcache filled by
	 * RelationBuildLocalRelation() can be relied upon and that the relation's
	 * catalog entries will either not yet exist (new relation), or will still
	 * reference the old relfilenode.
	 *
	 * As output *freezeXid, *minmulti must be set to the values appropriate
	 * for pg_class.{relfrozenxid, relminmxid}. For AMs that don't need those
	 * fields to be filled they can be set to InvalidTransactionId and
	 * InvalidMultiXactId, respectively.
	 *
	 * See also table_relation_set_new_filenode().
	 */
	void		(*relation_set_new_filenode) (Relation rel,
											  const RelFileNode *newrnode,
											  char persistence,
											  TransactionId *freezeXid,
											  MultiXactId *minmulti);

	/*
	 * This callback needs to remove all contents from `rel`'s current
	 * relfilenode. No provisions for transactional behaviour need to be made.
	 * Often this can be implemented by truncating the underlying storage to
	 * its minimal size.
	 *
	 * See also table_relation_nontransactional_truncate().
	 */
	void		(*relation_nontransactional_truncate) (Relation rel);

	/*
	 * See table_relation_copy_data().
	 *
	 * This can typically be implemented by directly copying the underlying
	 * storage, unless it contains references to the tablespace internally.
	 */
	void		(*relation_copy_data) (Relation rel,
									   const RelFileNode *newrnode);

	/* See table_relation_copy_for_cluster() */
	void		(*relation_copy_for_cluster) (Relation NewTable,
											  Relation OldTable,
											  Relation OldIndex,
											  bool use_sort,
											  TransactionId OldestXmin,
											  TransactionId *xid_cutoff,
											  MultiXactId *multi_cutoff,
											  double *num_tuples,
											  double *tups_vacuumed,
											  double *tups_recently_dead);

	/*
	 * React to VACUUM command on the relation. The VACUUM can be triggered by
	 * a user or by autovacuum. The specific actions performed by the AM will
	 * depend heavily on the individual AM.
	 *
	 * On entry a transaction is already established, and the relation is
	 * locked with a ShareUpdateExclusive lock.
	 *
	 * Note that neither VACUUM FULL (and CLUSTER), nor ANALYZE go through
	 * this routine, even if (for ANALYZE) it is part of the same VACUUM
	 * command.
	 *
	 * There probably, in the future, needs to be a separate callback to
	 * integrate with autovacuum's scheduling.
	 */
	void		(*relation_vacuum) (Relation rel,
									struct VacuumParams *params,
									BufferAccessStrategy bstrategy);

	/*
	 * Prepare to analyze block `blockno` of `scan`. The scan has been started
	 * with table_beginscan_analyze().  See also
	 * table_scan_analyze_next_block().
	 *
	 * The callback may acquire resources like locks that are held until
	 * table_scan_analyze_next_tuple() returns false. It e.g. can make sense
	 * to hold a lock until all tuples on a block have been analyzed by
	 * scan_analyze_next_tuple.
	 *
	 * The callback can return false if the block is not suitable for
	 * sampling, e.g. because it's a metapage that could never contain tuples.
	 *
	 * XXX: This obviously is primarily suited for block-based AMs. It's not
	 * clear what a good interface for non block based AMs would be, so there
	 * isn't one yet.
	 */
	bool		(*scan_analyze_next_block) (TableScanDesc scan,
											BlockNumber blockno,
											BufferAccessStrategy bstrategy);

	/*
	 * See table_scan_analyze_next_tuple().
	 *
	 * Not every AM might have a meaningful concept of dead rows, in which
	 * case it's OK to not increment *deadrows - but note that that may
	 * influence autovacuum scheduling (see comment for relation_vacuum
	 * callback).
	 */
	bool		(*scan_analyze_next_tuple) (TableScanDesc scan,
											TransactionId OldestXmin,
											double *liverows,
											double *deadrows,
											TupleTableSlot *slot);

	/* see table_index_build_range_scan for reference about parameters */
	double		(*index_build_range_scan) (Relation table_rel,
										   Relation index_rel,
										   struct IndexInfo *index_info,
										   bool allow_sync,
										   bool anyvisible,
										   bool progress,
										   BlockNumber start_blockno,
										   BlockNumber numblocks,
										   IndexBuildCallback callback,
										   void *callback_state,
										   TableScanDesc scan);

	/* see table_index_validate_scan for reference about parameters */
	void		(*index_validate_scan) (Relation table_rel,
										Relation index_rel,
										struct IndexInfo *index_info,
										Snapshot snapshot,
										struct ValidateIndexState *state);


	/* ------------------------------------------------------------------------
	 * Miscellaneous functions.
	 * ------------------------------------------------------------------------
	 */

	/*
	 * See table_relation_size().
	 *
	 * Note that currently a few callers use the MAIN_FORKNUM size to figure
	 * out the range of potentially interesting blocks (brin, analyze). It's
	 * probable that we'll need to revise the interface for those at some
	 * point.
	 */
	uint64		(*relation_size) (Relation rel, ForkNumber forkNumber);


	/*
	 * This callback should return true if the relation requires a TOAST table
	 * and false if it does not.  It may wish to examine the relation's tuple
	 * descriptor before making a decision, but if it uses some other method
	 * of storing large values (or if it does not support them) it can simply
	 * return false.
	 */
	bool		(*relation_needs_toast_table) (Relation rel);

	/*
	 * This callback should return the OID of the table AM that implements
	 * TOAST tables for this AM.  If the relation_needs_toast_table callback
	 * always returns false, this callback is not required.
	 */
	Oid			(*relation_toast_am) (Relation rel);

	/*
	 * This callback is invoked when detoasting a value stored in a toast
	 * table implemented by this AM.  See table_relation_fetch_toast_slice()
	 * for more details.
	 */
	void		(*relation_fetch_toast_slice) (Relation toastrel, Oid valueid,
											   int32 attrsize,
											   int32 sliceoffset,
											   int32 slicelength,
											   struct varlena *result);


	/* ------------------------------------------------------------------------
	 * Planner related functions.
	 * ------------------------------------------------------------------------
	 */

	/*
	 * See table_relation_estimate_size().
	 *
	 * While block oriented, it shouldn't be too hard for an AM that doesn't
	 * internally use blocks to convert into a usable representation.
	 *
	 * This differs from the relation_size callback by returning size
	 * estimates (both relation size and tuple count) for planning purposes,
	 * rather than returning a currently correct estimate.
	 */
	void		(*relation_estimate_size) (Relation rel, int32 *attr_widths,
										   BlockNumber *pages, double *tuples,
										   double *allvisfrac);


	/* ------------------------------------------------------------------------
	 * Executor related functions.
	 * ------------------------------------------------------------------------
	 */

	/*
	 * Prepare to fetch / check / return tuples from `tbmres->blockno` as part
	 * of a bitmap table scan. `scan` was started via table_beginscan_bm().
	 * Return false if there are no tuples to be found on the page, true
	 * otherwise.
	 *
	 * This will typically read and pin the target block, and do the necessary
	 * work to allow scan_bitmap_next_tuple() to return tuples (e.g. it might
	 * make sense to perform tuple visibility checks at this time). For some
	 * AMs it will make more sense to do all the work referencing `tbmres`
	 * contents here, for others it might be better to defer more work to
	 * scan_bitmap_next_tuple.
	 *
	 * If `tbmres->blockno` is -1, this is a lossy scan and all visible tuples
	 * on the page have to be returned, otherwise the tuples at offsets in
	 * `tbmres->offsets` need to be returned.
	 *
	 * XXX: Currently this may only be implemented if the AM uses md.c as its
	 * storage manager, and uses ItemPointer->ip_blkid in a manner that maps
	 * blockids directly to the underlying storage. nodeBitmapHeapscan.c
	 * performs prefetching directly using that interface.  This probably
	 * needs to be rectified at a later point.
	 *
	 * XXX: Currently this may only be implemented if the AM uses the
	 * visibilitymap, as nodeBitmapHeapscan.c unconditionally accesses it to
	 * perform prefetching.  This probably needs to be rectified at a later
	 * point.
	 *
	 * Optional callback, but either both scan_bitmap_next_block and
	 * scan_bitmap_next_tuple need to exist, or neither.
	 */
	bool		(*scan_bitmap_next_block) (TableScanDesc scan,
										   struct TBMIterateResult *tbmres);

	/*
	 * Fetch the next tuple of a bitmap table scan into `slot` and return true
	 * if a visible tuple was found, false otherwise.
	 *
	 * For some AMs it will make more sense to do all the work referencing
	 * `tbmres` contents in scan_bitmap_next_block, for others it might be
	 * better to defer more work to this callback.
	 *
	 * Optional callback, but either both scan_bitmap_next_block and
	 * scan_bitmap_next_tuple need to exist, or neither.
	 */
	bool		(*scan_bitmap_next_tuple) (TableScanDesc scan,
										   struct TBMIterateResult *tbmres,
										   TupleTableSlot *slot);

	/*
	 * Prepare to fetch tuples from the next block in a sample scan. Return
	 * false if the sample scan is finished, true otherwise. `scan` was
	 * started via table_beginscan_sampling().
	 *
	 * Typically this will first determine the target block by calling the
	 * TsmRoutine's NextSampleBlock() callback if not NULL, or alternatively
	 * perform a sequential scan over all blocks.  The determined block is
	 * then typically read and pinned.
	 *
	 * As the TsmRoutine interface is block based, a block needs to be passed
	 * to NextSampleBlock(). If that's not appropriate for an AM, it
	 * internally needs to perform mapping between the internal and a block
	 * based representation.
	 *
	 * Note that it's not acceptable to hold deadlock prone resources such as
	 * lwlocks until scan_sample_next_tuple() has exhausted the tuples on the
	 * block - the tuple is likely to be returned to an upper query node, and
	 * the next call could be off a long while. Holding buffer pins and such
	 * is obviously OK.
	 *
	 * Currently it is required to implement this interface, as there's no
	 * alternative way (contrary e.g. to bitmap scans) to implement sample
	 * scans. If infeasible to implement, the AM may raise an error.
	 */
	bool		(*scan_sample_next_block) (TableScanDesc scan,
										   struct SampleScanState *scanstate);

	/*
	 * This callback, only called after scan_sample_next_block has returned
	 * true, should determine the next tuple to be returned from the selected
	 * block using the TsmRoutine's NextSampleTuple() callback.
	 *
	 * The callback needs to perform visibility checks, and only return
	 * visible tuples. That obviously can mean calling NextSampleTuple()
	 * multiple times.
	 *
	 * The TsmRoutine interface assumes that there's a maximum offset on a
	 * given page, so if that doesn't apply to an AM, it needs to emulate that
	 * assumption somehow.
	 */
	bool		(*scan_sample_next_tuple) (TableScanDesc scan,
										   struct SampleScanState *scanstate,
										   TupleTableSlot *slot);

} TableAmRoutine;


```