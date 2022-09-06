#1.struct JoinExpr

```cpp
/*----------
 * JoinExpr - for SQL JOIN expressions
 *
 * isNatural, using, and quals are interdependent.  The user can write only
 * one of NATURAL, USING(), or ON() (this is enforced by the grammar).
 * If he writes NATURAL then parse analysis generates the equivalent USING()
 * list, and from that fills in "quals" with the right equality comparisons.
 * If he writes USING() then "quals" is filled with equality comparisons.
 * If he writes ON() then only "quals" is set.  Note that NATURAL/USING
 * are not equivalent to ON() since they also affect the output column list.
 *
 * alias is an Alias node representing the AS alias-clause attached to the
 * join expression, or NULL if no clause.  NB: presence or absence of the
 * alias has a critical impact on semantics, because a join with an alias
 * restricts visibility of the tables/columns inside it.
 *
 * During parse analysis, an RTE is created for the Join, and its index
 * is filled into rtindex.  This RTE is present mainly so that Vars can
 * be created that refer to the outputs of the join.  The planner sometimes
 * generates JoinExprs internally; these can have rtindex = 0 if there are
 * no join alias variables referencing such joins.
 *----------
 */
typedef struct JoinExpr
{
    NodeTag     type;
    JoinType    jointype;       /* type of join */
    bool        isNatural;      /* Natural join? Will need to shape table */
    Node       *larg;           /* left subtree */
    Node       *rarg;           /* right subtree */
    List       *using;          /* USING clause, if any (list of String) */
    Node       *quals;          /* qualifiers on join, if any */
    Alias      *alias;          /* user-written alias clause, if any */
    int         rtindex;        /* RT index assigned for join, or 0 */
} JoinExpr;

```