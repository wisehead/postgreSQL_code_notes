#1.struct PartitionPruneState

```cpp
/*
 * PartitionPruneState - State object required for plan nodes to perform
 * run-time partition pruning.
 *
 * This struct can be attached to plan types which support arbitrary Lists of
 * subplans containing partitions, to allow subplans to be eliminated due to
 * the clauses being unable to match to any tuple that the subplan could
 * possibly produce.
 *
 * execparamids			Contains paramids of PARAM_EXEC Params found within
 *						any of the partprunedata structs.  Pruning must be
 *						done again each time the value of one of these
 *						parameters changes.
 * other_subplans		Contains indexes of subplans that don't belong to any
 *						"partprunedata", e.g UNION ALL children that are not
 *						partitioned tables, or a partitioned table that the
 *						planner deemed run-time pruning to be useless for.
 *						These must not be pruned.
 * prune_context		A short-lived memory context in which to execute the
 *						partition pruning functions.
 * do_initial_prune		true if pruning should be performed during executor
 *						startup (at any hierarchy level).
 * do_exec_prune		true if pruning should be performed during
 *						executor run (at any hierarchy level).
 * num_partprunedata	Number of items in "partprunedata" array.
 * partprunedata		Array of PartitionPruningData pointers for the plan's
 *						partitioned relation(s), one for each partitioning
 *						hierarchy that requires run-time pruning.
 */
typedef struct PartitionPruneState
{
	Bitmapset  *execparamids;
	Bitmapset  *other_subplans;
	MemoryContext prune_context;
	bool		do_initial_prune;
	bool		do_exec_prune;
	int			num_partprunedata;
	PartitionPruningData *partprunedata[FLEXIBLE_ARRAY_MEMBER];
} PartitionPruneState;

```