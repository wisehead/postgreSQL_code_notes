#1.struct PlannedStmt

```cpp
/* ----------------
 *      PlannedStmt node
 *
 * The output of the planner is a Plan tree headed by a PlannedStmt node.
 * PlannedStmt holds the "one time" information needed by the executor.
 * ----------------
 */
typedef struct PlannedStmt
{
    NodeTag     type;

    CmdType     commandType;    /* select|insert|update|delete */

    bool        canSetTag;      /* do I set the command result tag? */

    bool        transientPlan;  /* redo plan when TransactionXmin changes? */

    struct Plan *planTree;      /* tree of Plan nodes */

    List       *rtable;         /* list of RangeTblEntry nodes */

    /* rtable indexes of target relations for INSERT/UPDATE/DELETE */
    List       *resultRelations;    /* integer list of RT indexes, or NIL */

    Node       *utilityStmt;    /* non-null if this is DECLARE CURSOR */

    IntoClause *intoClause;     /* target for SELECT INTO / CREATE TABLE AS */

    List       *subplans;       /* Plan trees for SubPlan expressions */

    Bitmapset  *rewindPlanIDs;  /* indices of subplans that require REWIND */

    /*
     * If the query has a returningList then the planner will store a list of
     * processed targetlists (one per result relation) here.  We must have a
     * separate RETURNING targetlist for each result rel because column
     * numbers may vary within an inheritance tree.  In the targetlists, Vars
     * referencing the result relation will have their original varno and
     * varattno, while Vars referencing other rels will be converted to have
     * varno OUTER and varattno referencing a resjunk entry in the top plan
     * node's targetlist.
     */
    List       *returningLists; /* list of lists of TargetEntry, or NIL */

    List       *rowMarks;       /* a list of RowMarkClause's */

    List       *relationOids;   /* OIDs of relations the plan depends on */

    List       *invalItems;     /* other dependencies, as PlanInvalItems */

    int         nParamExec;     /* number of PARAM_EXEC Params used */
} PlannedStmt;
```