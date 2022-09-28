#1.struct EPQState

```cpp
/*
 * EPQState is state for executing an EvalPlanQual recheck on a candidate
 * tuples e.g. in ModifyTable or LockRows.
 *
 * To execute EPQ a separate EState is created (stored in ->recheckestate),
 * which shares some resources, like the rangetable, with the main query's
 * EState (stored in ->parentestate). The (sub-)tree of the plan that needs to
 * be rechecked (in ->plan), is separately initialized (into
 * ->recheckplanstate), but shares plan nodes with the corresponding nodes in
 * the main query. The scan nodes in that separate executor tree are changed
 * to return only the current tuple of interest for the respective
 * table. Those tuples are either provided by the caller (using
 * EvalPlanQualSlot), and/or found using the rowmark mechanism (non-locking
 * rowmarks by the EPQ machinery itself, locking ones by the caller).
 *
 * While the plan to be checked may be changed using EvalPlanQualSetPlan(),
 * all such plans need to share the same EState.
 */
typedef struct EPQState
{
	/* Initialized at EvalPlanQualInit() time: */

	EState	   *parentestate;	/* main query's EState */
	int			epqParam;		/* ID of Param to force scan node re-eval */

	/*
	 * Tuples to be substituted by scan nodes. They need to set up, before
	 * calling EvalPlanQual()/EvalPlanQualNext(), into the slot returned by
	 * EvalPlanQualSlot(scanrelid). The array is indexed by scanrelid - 1.
	 */
	List	   *tuple_table;	/* tuple table for relsubs_slot */
	TupleTableSlot **relsubs_slot;

	/*
	 * Initialized by EvalPlanQualInit(), may be changed later with
	 * EvalPlanQualSetPlan():
	 */

	Plan	   *plan;			/* plan tree to be executed */
	List	   *arowMarks;		/* ExecAuxRowMarks (non-locking only) */


	/*
	 * The original output tuple to be rechecked.  Set by
	 * EvalPlanQualSetSlot(), before EvalPlanQualNext() or EvalPlanQual() may
	 * be called.
	 */
	TupleTableSlot *origslot;


	/* Initialized or reset by EvalPlanQualBegin(): */

	EState	   *recheckestate;	/* EState for EPQ execution, see above */

	/*
	 * Rowmarks that can be fetched on-demand using
	 * EvalPlanQualFetchRowMark(), indexed by scanrelid - 1. Only non-locking
	 * rowmarks.
	 */
	ExecAuxRowMark **relsubs_rowmark;

	/*
	 * True if a relation's EPQ tuple has been fetched for relation, indexed
	 * by scanrelid - 1.
	 */
	bool	   *relsubs_done;

	PlanState  *recheckplanstate;	/* EPQ specific exec nodes, for ->plan */
} EPQState;

```