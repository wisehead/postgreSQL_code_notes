#1.struct QueryDesc

```cpp

/* ----------------
 *      query descriptor:
 *
 *  a QueryDesc encapsulates everything that the executor
 *  needs to execute the query.
 *
 *  For the convenience of SQL-language functions, we also support QueryDescs
 *  containing utility statements; these must not be passed to the executor
 *  however.
 * ---------------------
 */
typedef struct QueryDesc
{
    /* These fields are provided by CreateQueryDesc */
    CmdType     operation;      /* CMD_SELECT, CMD_UPDATE, etc. */
    PlannedStmt *plannedstmt;   /* planner's output, or null if utility */
    Node       *utilitystmt;    /* utility statement, or null */
    const char *sourceText;     /* source text of the query */
    Snapshot    snapshot;       /* snapshot to use for query */
    Snapshot    crosscheck_snapshot;    /* crosscheck for RI update/delete */
    DestReceiver *dest;         /* the destination for tuple output */
    ParamListInfo params;       /* param values being passed in */
    bool        doInstrument;   /* TRUE requests runtime instrumentation */

    /* These fields are set by ExecutorStart */
    TupleDesc   tupDesc;        /* descriptor for result tuples */
    EState     *estate;         /* executor's query-wide state */
    PlanState  *planstate;      /* tree of per-plan-node state */

    /* This is always set NULL by the core system, but plugins can change it */
    struct Instrumentation *totaltime;  /* total time spent in ExecutorRun */
} QueryDesc;
```