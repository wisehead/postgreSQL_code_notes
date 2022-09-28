#1.struct Append

```cpp
/* ----------------
 *	 Append node -
 *		Generate the concatenation of the results of sub-plans.
 * ----------------
 */
typedef struct Append
{
	Plan		plan;
	Bitmapset  *apprelids;		/* RTIs of appendrel(s) formed by this node */
	List	   *appendplans;
	int			nasyncplans;	/* # of asynchronous plans */

	/*
	 * All 'appendplans' preceding this index are non-partial plans. All
	 * 'appendplans' from this index onwards are partial plans.
	 */
	int			first_partial_plan;

	/* Info for run-time subplan pruning; NULL if we're not doing that */
	struct PartitionPruneInfo *part_prune_info;
} Append;

```