#1.struct PlanState


```cpp
/* ----------------
 *      PlanState node
 *
 * We never actually instantiate any PlanState nodes; this is just the common
 * abstract superclass for all PlanState-type nodes.
 * ----------------
 */
typedef struct PlanState
{
    NodeTag     type;

    Plan       *plan;           /* associated Plan node */

    EState     *state;          /* at execution time, states of individual
                                 * nodes point to one EState for the whole
                                 * top-level plan */

    struct Instrumentation *instrument; /* Optional runtime stats for this
                                         * plan node */

    /*
     * Common structural data for all Plan types.  These links to subsidiary
     * state trees parallel links in the associated plan tree (except for the
     * subPlan list, which does not exist in the plan tree).
     */
    List       *targetlist;     /* target list to be computed at this node */
    List       *qual;           /* implicitly-ANDed qual conditions */
    struct PlanState *lefttree; /* input plan tree(s) */
    struct PlanState *righttree;
    List       *initPlan;       /* Init SubPlanState nodes (un-correlated expr
                                 * subselects) */
    List       *subPlan;        /* SubPlanState nodes in my expressions */

    /*
     * State for management of parameter-change-driven rescanning
     */
    Bitmapset  *chgParam;       /* set of IDs of changed Params */

    /*
     * Other run-time state needed by most if not all node types.
     */
    TupleTableSlot *ps_ResultTupleSlot; /* slot for my result tuples */
    ExprContext *ps_ExprContext;    /* node's expression-evaluation context */
    ProjectionInfo *ps_ProjInfo;    /* info for doing tuple projection */
    bool        ps_TupFromTlist;/* state flag for processing set-valued
                                 * functions in targetlist */
} PlanState;
```