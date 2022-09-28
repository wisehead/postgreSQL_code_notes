#1.struct AppendState
```cpp
struct AppendState
{
	PlanState	ps;				/* its first field is NodeTag */
	PlanState **appendplans;	/* array of PlanStates for my inputs */
	int			as_nplans;
	int			as_whichplan;
	bool		as_begun;		/* false means need to initialize */
	Bitmapset  *as_asyncplans;	/* asynchronous plans indexes */
	int			as_nasyncplans; /* # of asynchronous plans */
	AsyncRequest **as_asyncrequests;	/* array of AsyncRequests */
	TupleTableSlot **as_asyncresults;	/* unreturned results of async plans */
	int			as_nasyncresults;	/* # of valid entries in as_asyncresults */
	bool		as_syncdone;	/* true if all synchronous plans done in
								 * asynchronous mode, else false */
	int			as_nasyncremain;	/* # of remaining asynchronous plans */
	Bitmapset  *as_needrequest; /* asynchronous plans needing a new request */
	struct WaitEventSet *as_eventset;	/* WaitEventSet used to configure file
										 * descriptor wait events */
	int			as_first_partial_plan;	/* Index of 'appendplans' containing
										 * the first partial plan */
	ParallelAppendState *as_pstate; /* parallel coordination info */
	Size		pstate_len;		/* size of parallel coordination info */
	struct PartitionPruneState *as_prune_state;
	Bitmapset  *as_valid_subplans;
	Bitmapset  *as_valid_asyncplans;	/* valid asynchronous plans indexes */
	bool		(*choose_next_subplan) (AppendState *);
};
```