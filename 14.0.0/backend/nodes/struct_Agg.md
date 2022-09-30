#1.struct Agg

```cpp
/* ---------------
 *		aggregate node
 *
 * An Agg node implements plain or grouped aggregation.  For grouped
 * aggregation, we can work with presorted input or unsorted input;
 * the latter strategy uses an internal hashtable.
 *
 * Notice the lack of any direct info about the aggregate functions to be
 * computed.  They are found by scanning the node's tlist and quals during
 * executor startup.  (It is possible that there are no aggregate functions;
 * this could happen if they get optimized away by constant-folding, or if
 * we are using the Agg node to implement hash-based grouping.)
 * ---------------
 */
typedef struct Agg
{
	Plan		plan;
	AggStrategy aggstrategy;	/* basic strategy, see nodes.h */
	AggSplit	aggsplit;		/* agg-splitting mode, see nodes.h */
	int			numCols;		/* number of grouping columns */
	AttrNumber *grpColIdx;		/* their indexes in the target list */
	Oid		   *grpOperators;	/* equality operators to compare with */
	Oid		   *grpCollations;
	long		numGroups;		/* estimated number of groups in input */
	uint64		transitionSpace;	/* for pass-by-ref transition data */
	Bitmapset  *aggParams;		/* IDs of Params used in Aggref inputs */
	/* Note: planner provides numGroups & aggParams only in HASHED/MIXED case */
	List	   *groupingSets;	/* grouping sets to use */
	List	   *chain;			/* chained Agg/Sort nodes */
} Agg;
```