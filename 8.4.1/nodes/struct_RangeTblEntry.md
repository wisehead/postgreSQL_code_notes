#1.struct RangeTblEntry

```cpp
typedef struct RangeTblEntry
{
    NodeTag     type;

    RTEKind     rtekind;        /* see above */

    /*
     * XXX the fields applicable to only some rte kinds should be merged into
     * a union.  I didn't do this yet because the diffs would impact a lot of
     * code that is being actively worked on.  FIXME someday.
     */

    /*
     * Fields valid for a plain relation RTE (else zero):
     */
    Oid         relid;          /* OID of the relation */

    /*
     * Fields valid for a subquery RTE (else NULL):
     */
    Query      *subquery;       /* the sub-query */

    /*
     * Fields valid for a join RTE (else NULL/zero):
     *
     * joinaliasvars is a list of (usually) Vars corresponding to the columns
     * of the join result.  An alias Var referencing column K of the join
     * result can be replaced by the K'th element of joinaliasvars --- but to
     * simplify the task of reverse-listing aliases correctly, we do not do
     * that until planning time.  In detail: an element of joinaliasvars can
     * be a Var of one of the join's input relations, or such a Var with an
     * implicit coercion to the join's output column type, or a COALESCE
     * expression containing the two input column Vars (possibly coerced).
     * Within a Query loaded from a stored rule, it is also possible for
     * joinaliasvars items to be null pointers, which are placeholders for
     * (necessarily unreferenced) columns dropped since the rule was made.
     * Also, once planning begins, joinaliasvars items can be almost anything,
     * as a result of subquery-flattening substitutions.
     */
    JoinType    jointype;       /* type of join */
    List       *joinaliasvars;  /* list of alias-var expansions */

    /*
     * Fields valid for a function RTE (else NULL):
     *
     * If the function returns RECORD, funccoltypes lists the column types
     * declared in the RTE's column type specification, and funccoltypmods
     * lists their declared typmods.  Otherwise, both fields are NIL.
     */
    Node       *funcexpr;       /* expression tree for func call */
    List       *funccoltypes;   /* OID list of column type OIDs */
    List       *funccoltypmods; /* integer list of column typmods */

    /*
     * Fields valid for a values RTE (else NIL):
     */
    List       *values_lists;   /* list of expression lists */

    /*
     * Fields valid for a CTE RTE (else NULL/zero):
     */
    char       *ctename;        /* name of the WITH list item */
    Index       ctelevelsup;    /* number of query levels up */
    bool        self_reference; /* is this a recursive self-reference? */
    List       *ctecoltypes;    /* OID list of column type OIDs */
    List       *ctecoltypmods;  /* integer list of column typmods */

    /*
     * Fields valid in all RTEs:
     */
    Alias      *alias;          /* user-written alias clause, if any */
    Alias      *eref;           /* expanded reference names */
    bool        inh;            /* inheritance requested? */
    bool        inFromCl;       /* present in FROM clause? */
    AclMode     requiredPerms;  /* bitmask of required access permissions */
    Oid         checkAsUser;    /* if valid, check access as this role */
    Bitmapset  *selectedCols;   /* columns needing SELECT permission */
    Bitmapset  *modifiedCols;   /* columns needing INSERT/UPDATE permission */
} RangeTblEntry;

```