#1.struct EState


```cpp
/* ----------------
 *    EState information
 *
 * Master working state for an Executor invocation
 * ----------------
 */
typedef struct EState
{
    NodeTag     type;

    /* Basic state for all query types: */
    ScanDirection es_direction; /* current scan direction */
    Snapshot    es_snapshot;    /* time qual to use */
    Snapshot    es_crosscheck_snapshot; /* crosscheck time qual for RI */
    List       *es_range_table; /* List of RangeTblEntry */

    /* If query can insert/delete tuples, the command ID to mark them with */
    CommandId   es_output_cid;

    /* Info about target table for insert/update/delete queries: */
    ResultRelInfo *es_result_relations; /* array of ResultRelInfos */
    int         es_num_result_relations;        /* length of array */
    ResultRelInfo *es_result_relation_info;     /* currently active array elt */
    JunkFilter *es_junkFilter;  /* currently active junk filter */

    /* Stuff used for firing triggers: */
    List       *es_trig_target_relations;       /* trigger-only ResultRelInfos */
    TupleTableSlot *es_trig_tuple_slot; /* for trigger output tuples */

    /* Parameter info: */
    ParamListInfo es_param_list_info;   /* values of external params */
    ParamExecData *es_param_exec_vals;  /* values of internal params */

    /* Other working state: */
    MemoryContext es_query_cxt; /* per-query context in which EState lives */

    TupleTable  es_tupleTable;  /* Array of TupleTableSlots */

    uint32      es_processed;   /* # of tuples processed */
    Oid         es_lastoid;     /* last oid processed (by INSERT) */
    List       *es_rowMarks;    /* not good place, but there is no other */

    bool        es_instrument;  /* true requests runtime instrumentation */
    bool        es_select_into; /* true if doing SELECT INTO */
    bool        es_into_oids;   /* true to generate OIDs in SELECT INTO */
    List       *es_exprcontexts;    /* List of ExprContexts within EState */

    List       *es_subplanstates;       /* List of PlanState for SubPlans */

    /*
     * this ExprContext is for per-output-tuple operations, such as constraint
     * checks and index-value computations.  It will be reset for each output
     * tuple.  Note that it will be created only if needed.
     */
    ExprContext *es_per_tuple_exprcontext;

    /* Below is to re-evaluate plan qual in READ COMMITTED mode */
    PlannedStmt *es_plannedstmt;    /* link to top of plan tree */
    struct evalPlanQual *es_evalPlanQual;       /* chain of PlanQual states */
    bool       *es_evTupleNull; /* local array of EPQ status */
    HeapTuple  *es_evTuple;     /* shared array of EPQ substitute tuples */
    bool        es_useEvalPlan; /* evaluating EPQ tuples? */
} EState;    

```