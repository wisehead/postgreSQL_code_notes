#1.struct PortalData

```cpp
typedef struct PortalData
{
	/* Bookkeeping data */
	const char *name;			/* portal's name */
	const char *prepStmtName;	/* source prepared statement (NULL if none) */
	MemoryContext portalContext;	/* subsidiary memory for portal */
	ResourceOwner resowner;		/* resources owned by portal */
	void		(*cleanup) (Portal portal); /* cleanup hook */

	/*
	 * State data for remembering which subtransaction(s) the portal was
	 * created or used in.  If the portal is held over from a previous
	 * transaction, both subxids are InvalidSubTransactionId.  Otherwise,
	 * createSubid is the creating subxact and activeSubid is the last subxact
	 * in which we ran the portal.
	 */
	SubTransactionId createSubid;	/* the creating subxact */
	SubTransactionId activeSubid;	/* the last subxact with activity */

	/* The query or queries the portal will execute */
	const char *sourceText;		/* text of query (as of 8.4, never NULL) */
	CommandTag	commandTag;		/* command tag for original query */
	QueryCompletion qc;			/* command completion data for executed query */
	List	   *stmts;			/* list of PlannedStmts */
	CachedPlan *cplan;			/* CachedPlan, if stmts are from one */

	ParamListInfo portalParams; /* params to pass to query */
	QueryEnvironment *queryEnv; /* environment for query */

	/* Features/options */
	PortalStrategy strategy;	/* see above */
	int			cursorOptions;	/* DECLARE CURSOR option bits */
	bool		run_once;		/* portal will only be run once */

	/* Status data */
	PortalStatus status;		/* see above */
	bool		portalPinned;	/* a pinned portal can't be dropped */
	bool		autoHeld;		/* was automatically converted from pinned to
								 * held (see HoldPinnedPortals()) */

	/* If not NULL, Executor is active; call ExecutorEnd eventually: */
	QueryDesc  *queryDesc;		/* info needed for executor invocation */

	/* If portal returns tuples, this is their tupdesc: */
	TupleDesc	tupDesc;		/* descriptor for result tuples */
	/* and these are the format codes to use for the columns: */
	int16	   *formats;		/* a format code for each column */

	/*
	 * Outermost ActiveSnapshot for execution of the portal's queries.  For
	 * all but a few utility commands, we require such a snapshot to exist.
	 * This ensures that TOAST references in query results can be detoasted,
	 * and helps to reduce thrashing of the process's exposed xmin.
	 */
	Snapshot	portalSnapshot; /* active snapshot, or NULL if none */

	/*
	 * Where we store tuples for a held cursor or a PORTAL_ONE_RETURNING or
	 * PORTAL_UTIL_SELECT query.  (A cursor held past the end of its
	 * transaction no longer has any active executor state.)
	 */
	Tuplestorestate *holdStore; /* store for holdable cursors */
	MemoryContext holdContext;	/* memory containing holdStore */

	/*
	 * Snapshot under which tuples in the holdStore were read.  We must keep a
	 * reference to this snapshot if there is any possibility that the tuples
	 * contain TOAST references, because releasing the snapshot could allow
	 * recently-dead rows to be vacuumed away, along with any toast data
	 * belonging to them.  In the case of a held cursor, we avoid needing to
	 * keep such a snapshot by forcibly detoasting the data.
	 */
	Snapshot	holdSnapshot;	/* registered snapshot, or NULL if none */

	/*
	 * atStart, atEnd and portalPos indicate the current cursor position.
	 * portalPos is zero before the first row, N after fetching N'th row of
	 * query.  After we run off the end, portalPos = # of rows in query, and
	 * atEnd is true.  Note that atStart implies portalPos == 0, but not the
	 * reverse: we might have backed up only as far as the first row, not to
	 * the start.  Also note that various code inspects atStart and atEnd, but
	 * only the portal movement routines should touch portalPos.
	 */
	bool		atStart;
	bool		atEnd;
	uint64		portalPos;

	/* Presentation data, primarily used by the pg_cursors system view */
	TimestampTz creation_time;	/* time at which this portal was defined */
	bool		visible;		/* include this portal in pg_cursors? */
}			PortalData;


```