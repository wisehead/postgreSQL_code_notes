#1.struct Var


```cpp
typedef struct Var
{
    Expr        xpr;
    Index       varno;          /* index of this var's relation in the range
                                 * table (could also be INNER or OUTER) */
    AttrNumber  varattno;       /* attribute number of this var, or zero for
                                 * all */
    Oid         vartype;        /* pg_type OID for the type of this var */
    int32       vartypmod;      /* pg_attribute typmod value */
    Index       varlevelsup;    /* for subquery variables referencing outer
                                 * relations; 0 in a normal var, >0 means N
                                 * levels up */
    Index       varnoold;       /* original value of varno, for debugging */
    AttrNumber  varoattno;      /* original value of varattno */
    int         location;       /* token location, or -1 if unknown */
} Var;

```