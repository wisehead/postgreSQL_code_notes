#1.struct AggState

```cpp
/* ---------------------
 *	AggState information
 *
 *	ss.ss_ScanTupleSlot refers to output of underlying plan.
 *
 *	Note: ss.ps.ps_ExprContext contains ecxt_aggvalues and
 *	ecxt_aggnulls arrays, which hold the computed agg values for the current
 *	input group during evaluation of an Agg node's output tuple(s).  We
 *	create a second ExprContext, tmpcontext, in which to evaluate input
 *	expressions and run the aggregate transition functions.
 * ---------------------
 */
 
typedef struct AggState
{
	ScanState	ss;				/* its first field is NodeTag */
	List	   *aggs;			/* all Aggref nodes in targetlist & quals */
	int			numaggs;		/* length of list (could be zero!) */
	int			numtrans;		/* number of pertrans items */
	AggStrategy aggstrategy;	/* strategy mode */
	AggSplit	aggsplit;		/* agg-splitting mode, see nodes.h */
	AggStatePerPhase phase;		/* pointer to current phase data */
	int			numphases;		/* number of phases (including phase 0) */
	int			current_phase;	/* current phase number */
	AggStatePerAgg peragg;		/* per-Aggref information */
	AggStatePerTrans pertrans;	/* per-Trans state information */
	ExprContext *hashcontext;	/* econtexts for long-lived data (hashtable) */
	ExprContext **aggcontexts;	/* econtexts for long-lived data (per GS) */
	ExprContext *tmpcontext;	/* econtext for input expressions */
#define FIELDNO_AGGSTATE_CURAGGCONTEXT 14
	ExprContext *curaggcontext; /* currently active aggcontext */
	AggStatePerAgg curperagg;	/* currently active aggregate, if any */
#define FIELDNO_AGGSTATE_CURPERTRANS 16
	AggStatePerTrans curpertrans;	/* currently active trans state, if any */
	bool		input_done;		/* indicates end of input */
	bool		agg_done;		/* indicates completion of Agg scan */
	int			projected_set;	/* The last projected grouping set */
#define FIELDNO_AGGSTATE_CURRENT_SET 20
	int			current_set;	/* The current grouping set being evaluated */
	Bitmapset  *grouped_cols;	/* grouped cols in current projection */
	List	   *all_grouped_cols;	/* list of all grouped cols in DESC order */
	Bitmapset  *colnos_needed;	/* all columns needed from the outer plan */
	int			max_colno_needed;	/* highest colno needed from outer plan */
	bool		all_cols_needed;	/* are all cols from outer plan needed? */
	/* These fields are for grouping set phase data */
	int			maxsets;		/* The max number of sets in any phase */
	AggStatePerPhase phases;	/* array of all phases */
	Tuplesortstate *sort_in;	/* sorted input to phases > 1 */
	Tuplesortstate *sort_out;	/* input is copied here for next phase */
	TupleTableSlot *sort_slot;	/* slot for sort results */
	/* these fields are used in AGG_PLAIN and AGG_SORTED modes: */
	AggStatePerGroup *pergroups;	/* grouping set indexed array of per-group
									 * pointers */
	HeapTuple	grp_firstTuple; /* copy of first tuple of current group */
	/* these fields are used in AGG_HASHED and AGG_MIXED modes: */
	bool		table_filled;	/* hash table filled yet? */
	int			num_hashes;
	MemoryContext hash_metacxt; /* memory for hash table itself */
	struct HashTapeInfo *hash_tapeinfo; /* metadata for spill tapes */
	struct HashAggSpill *hash_spills;	/* HashAggSpill for each grouping set,
										 * exists only during first pass */
	TupleTableSlot *hash_spill_rslot;	/* for reading spill files */
	TupleTableSlot *hash_spill_wslot;	/* for writing spill files */
	List	   *hash_batches;	/* hash batches remaining to be processed */
	bool		hash_ever_spilled;	/* ever spilled during this execution? */
	bool		hash_spill_mode;	/* we hit a limit during the current batch
									 * and we must not create new groups */
	Size		hash_mem_limit; /* limit before spilling hash table */
	uint64		hash_ngroups_limit; /* limit before spilling hash table */
	int			hash_planned_partitions;	/* number of partitions planned
											 * for first pass */
	double		hashentrysize;	/* estimate revised during execution */
	Size		hash_mem_peak;	/* peak hash table memory usage */
	uint64		hash_ngroups_current;	/* number of groups currently in
										 * memory in all hash tables */
	uint64		hash_disk_used; /* kB of disk space used */
	int			hash_batches_used;	/* batches used during entire execution */

	AggStatePerHash perhash;	/* array of per-hashtable data */
	AggStatePerGroup *hash_pergroup;	/* grouping set indexed array of
										 * per-group pointers */

	/* support for evaluation of agg input expressions: */
#define FIELDNO_AGGSTATE_ALL_PERGROUPS 53
	AggStatePerGroup *all_pergroups;	/* array of first ->pergroups, than
										 * ->hash_pergroup */
	ProjectionInfo *combinedproj;	/* projection machinery */
	SharedAggInfo *shared_info; /* one entry per worker */
} AggState;
```