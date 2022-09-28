#1.struct ScanState

```cpp
/* ----------------
 *	 ScanState information
 *
 *		ScanState extends PlanState for node types that represent
 *		scans of an underlying relation.  It can also be used for nodes
 *		that scan the output of an underlying plan node --- in that case,
 *		only ScanTupleSlot is actually useful, and it refers to the tuple
 *		retrieved from the subplan.
 *
 *		currentRelation    relation being scanned (NULL if none)
 *		currentScanDesc    current scan descriptor for scan (NULL if none)
 *		ScanTupleSlot	   pointer to slot in tuple table holding scan tuple
 * ----------------
 */
typedef struct ScanState
{
	PlanState	ps;				/* its first field is NodeTag */
	Relation	ss_currentRelation;
	struct TableScanDescData *ss_currentScanDesc;
	TupleTableSlot *ss_ScanTupleSlot;
} ScanState;

```