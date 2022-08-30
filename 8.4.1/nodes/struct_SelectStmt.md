#1.struct SelectStmt

```cpp

typedef struct SelectStmt
{
    NodeTag     type;

    /*
     * These fields are used only in "leaf" SelectStmts.
     */
    List       *distinctClause; /* NULL, list of DISTINCT ON exprs, or
                                 * lcons(NIL,NIL) for all (SELECT DISTINCT) */
    IntoClause *intoClause;     /* target for SELECT INTO / CREATE TABLE AS */
    List       *targetList;     /* the target list (of ResTarget) */
    List       *fromClause;     /* the FROM clause */
    Node       *whereClause;    /* WHERE qualification */
    List       *groupClause;    /* GROUP BY clauses */
    Node       *havingClause;   /* HAVING conditional-expression */
    List       *windowClause;   /* WINDOW window_name AS (...), ... */
    WithClause *withClause;     /* WITH clause */

    /*
     * In a "leaf" node representing a VALUES list, the above fields are all
     * null, and instead this field is set.  Note that the elements of the
     * sublists are just expressions, without ResTarget decoration. Also note
     * that a list element can be DEFAULT (represented as a SetToDefault
     * node), regardless of the context of the VALUES list. It's up to parse
     * analysis to reject that where not valid.
     */
    List       *valuesLists;    /* untransformed list of expression lists */

    /*
     * These fields are used in both "leaf" SelectStmts and upper-level
     * SelectStmts.
     */
    List       *sortClause;     /* sort clause (a list of SortBy's) */
    Node       *limitOffset;    /* # of result tuples to skip */
    Node       *limitCount;     /* # of result tuples to return */
    List       *lockingClause;  /* FOR UPDATE (list of LockingClause's) */

    /*
     * These fields are used only in upper-level SelectStmts.
     */
    SetOperation op;            /* type of set op */
    bool        all;            /* ALL specified? */
    struct SelectStmt *larg;    /* left child */
    struct SelectStmt *rarg;    /* right child */
    /* Eventually add fields for CORRESPONDING spec here */
} SelectStmt;

```