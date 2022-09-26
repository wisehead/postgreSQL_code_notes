#1.struct EState

```
/* ----------------
 *	  EState information
 *
 * Working state for an Executor invocation
 * ----------------
 */
typedef struct EState
{
	NodeTag		type;

	/* Basic state for all query types: */
	ScanDirection es_direction; /* current scan direction */
	Snapshot	es_snapshot;	/* time qual to use */
	Snapshot	es_crosscheck_snapshot; /* crosscheck time qual for RI */
	List	   *es_range_table; /* List of RangeTblEntry */
	Index		es_range_table_size;	/* size of the range table arrays */
	Relation   *es_relations;	/* Array of per-range-table-entry Relation
								 * pointers, or NULL if not yet opened */
	struct ExecRowMark **es_rowmarks;	/* Array of per-range-table-entry
										 * ExecRowMarks, or NULL if none */
	PlannedStmt *es_plannedstmt;	/* link to top of plan tree */
	const char *es_sourceText;	/* Source text from QueryDesc */

	JunkFilter *es_junkFilter;	/* top-level junk filter, if any */

	/* If query can insert/delete tuples, the command ID to mark them with */
	CommandId	es_output_cid;

	/* Info about target table(s) for insert/update/delete queries: */
	ResultRelInfo **es_result_relations;	/* Array of per-range-table-entry
											 * ResultRelInfo pointers, or NULL
											 * if not a target table */
	List	   *es_opened_result_relations; /* List of non-NULL entries in
											 * es_result_relations in no
											 * specific order */

	PartitionDirectory es_partition_directory;	/* for PartitionDesc lookup */

	/*
	 * The following list contains ResultRelInfos created by the tuple routing
	 * code for partitions that aren't found in the es_result_relations array.
	 */
	List	   *es_tuple_routing_result_relations;

	/* Stuff used for firing triggers: */
	List	   *es_trig_target_relations;	/* trigger-only ResultRelInfos */

	/* Parameter info: */
	ParamListInfo es_param_list_info;	/* values of external params */
	ParamExecData *es_param_exec_vals;	/* values of internal params */

	QueryEnvironment *es_queryEnv;	/* query environment */

	/* Other working state: */
	MemoryContext es_query_cxt; /* per-query context in which EState lives */

	List	   *es_tupleTable;	/* List of TupleTableSlots */

	uint64		es_processed;	/* # of tuples processed */

	int			es_top_eflags;	/* eflags passed to ExecutorStart */
	int			es_instrument;	/* OR of InstrumentOption flags */
	bool		es_finished;	/* true when ExecutorFinish is done */

	List	   *es_exprcontexts;	/* List of ExprContexts within EState */

	List	   *es_subplanstates;	/* List of PlanState for SubPlans */

	List	   *es_auxmodifytables; /* List of secondary ModifyTableStates */

	/*
	 * this ExprContext is for per-output-tuple operations, such as constraint
	 * checks and index-value computations.  It will be reset for each output
	 * tuple.  Note that it will be created only if needed.
	 */
	ExprContext *es_per_tuple_exprcontext;

	/*
	 * If not NULL, this is an EPQState's EState. This is a field in EState
	 * both to allow EvalPlanQual aware executor nodes to detect that they
	 * need to perform EPQ related work, and to provide necessary information
	 * to do so.
	 */
	struct EPQState *es_epq_active;

	bool		es_use_parallel_mode;	/* can we use parallel workers? */

	/* The per-query shared memory area to use for parallel execution. */
	struct dsa_area *es_query_dsa;

	/*
	 * JIT information. es_jit_flags indicates whether JIT should be performed
	 * and with which options.  es_jit is created on-demand when JITing is
	 * performed.
	 *
	 * es_jit_worker_instr is the combined, on demand allocated,
	 * instrumentation from all workers. The leader's instrumentation is kept
	 * separate, and is combined on demand by ExplainPrintJITSummary().
	 */
	int			es_jit_flags;
	struct JitContext *es_jit;
	struct JitInstrumentation *es_jit_worker_instr;
} EState;

```