#1.struct Query

```cpp
/*****************************************************************************
 *  Query Tree
 *****************************************************************************/

/*
 * Query -
 *    Parse analysis turns all statements into a Query tree (via transformStmt)
 *    for further processing by the rewriter and planner.
 *
 *    Utility statements (i.e. non-optimizable statements) have the
 *    utilityStmt field set, and the Query itself is mostly dummy.
 *    DECLARE CURSOR is a special case: it is represented like a SELECT,
 *    but the original DeclareCursorStmt is stored in utilityStmt.
 *
 *    Planning converts a Query tree into a Plan tree headed by a PlannedStmt
 *    node --- the Query structure is not used by the executor.
 */

typedef struct Query
{
    NodeTag     type;

    CmdType     commandType;    /* select|insert|update|delete|utility */

    QuerySource querySource;    /* where did I come from? */

    bool        canSetTag;      /* do I set the command result tag? */

    Node       *utilityStmt;    /* non-null if this is DECLARE CURSOR or a
                                 * non-optimizable statement */

    int         resultRelation; /* rtable index of target relation for
                                 * INSERT/UPDATE/DELETE; 0 for SELECT */

    IntoClause *intoClause;     /* target for SELECT INTO / CREATE TABLE AS */

    bool        hasAggs;        /* has aggregates in tlist or havingQual */
    bool        hasWindowFuncs; /* has window functions in tlist */
    bool        hasSubLinks;    /* has subquery SubLink */
    bool        hasDistinctOn;  /* distinctClause is from DISTINCT ON */
    bool        hasRecursive;   /* WITH RECURSIVE was specified */

    List       *cteList;        /* WITH list (of CommonTableExpr's) */

    List       *rtable;         /* list of range table entries */
    FromExpr   *jointree;       /* table join tree (FROM and WHERE clauses) */

    List       *targetList;     /* target list (of TargetEntry) */

    List       *returningList;  /* return-values list (of TargetEntry) */

    List       *groupClause;    /* a list of SortGroupClause's */

    Node       *havingQual;     /* qualifications applied to groups */

    List       *windowClause;   /* a list of WindowClause's */

    List       *distinctClause; /* a list of SortGroupClause's */

    List       *sortClause;     /* a list of SortGroupClause's */

    Node       *limitOffset;    /* # of result tuples to skip (int8 expr) */
    Node       *limitCount;     /* # of result tuples to return (int8 expr) */

    List       *rowMarks;       /* a list of RowMarkClause's */

    Node       *setOperations;  /* set-operation tree if this is top level of
                                 * a UNION/INTERSECT/EXCEPT query */
} Query;

```