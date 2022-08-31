#1.struct PortalData

```cpp
typedef struct PortalData
{
    /* Bookkeeping data */
    const char *name;           /* portal's name */
    const char *prepStmtName;   /* source prepared statement (NULL if none) */
    MemoryContext heap;         /* subsidiary memory for portal */
    ResourceOwner resowner;     /* resources owned by portal */
    void        (*cleanup) (Portal portal);     /* cleanup hook */
    SubTransactionId createSubid;       /* the ID of the creating subxact */

    /*
     * if createSubid is InvalidSubTransactionId, the portal is held over from
     * a previous transaction
     */

    /* The query or queries the portal will execute */
    const char *sourceText;     /* text of query (as of 8.4, never NULL) */
    const char *commandTag;     /* command tag for original query */
    List       *stmts;          /* PlannedStmts and/or utility statements */
    CachedPlan *cplan;          /* CachedPlan, if stmts are from one */

    ParamListInfo portalParams; /* params to pass to query */

    /* Features/options */
    PortalStrategy strategy;    /* see above */
    int         cursorOptions;  /* DECLARE CURSOR option bits */

    /* Status data */
    PortalStatus status;        /* see above */
    bool        portalPinned;   /* a pinned portal can't be dropped */

    /* If not NULL, Executor is active; call ExecutorEnd eventually: */
    QueryDesc  *queryDesc;      /* info needed for executor invocation */

    /* If portal returns tuples, this is their tupdesc: */
    TupleDesc   tupDesc;        /* descriptor for result tuples */
    /* and these are the format codes to use for the columns: */
    int16      *formats;        /* a format code for each column */

    /*
     * Where we store tuples for a held cursor or a PORTAL_ONE_RETURNING or
     * PORTAL_UTIL_SELECT query.  (A cursor held past the end of its
     * transaction no longer has any active executor state.)
     */
    Tuplestorestate *holdStore; /* store for holdable cursors */
    MemoryContext holdContext;  /* memory containing holdStore */

    /*
     * atStart, atEnd and portalPos indicate the current cursor position.
     * portalPos is zero before the first row, N after fetching N'th row of
     * query.  After we run off the end, portalPos = # of rows in query, and
     * atEnd is true.  If portalPos overflows, set posOverflow (this causes us
     * to stop relying on its value for navigation).  Note that atStart
     * implies portalPos == 0, but not the reverse (portalPos could have
     * overflowed).
     */
    bool        atStart;
    bool        atEnd;
    bool        posOverflow;
    long        portalPos;

    /* Presentation data, primarily used by the pg_cursors system view */
    TimestampTz creation_time;  /* time at which this portal was defined */
    bool        visible;        /* include this portal in pg_cursors? */
} PortalData;
```