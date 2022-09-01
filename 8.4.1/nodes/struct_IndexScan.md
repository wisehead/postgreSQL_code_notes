#1.struct IndexScan

```cpp
/* ----------------
 *      index scan node
 *
 * indexqualorig is an implicitly-ANDed list of index qual expressions, each
 * in the same form it appeared in the query WHERE condition.  Each should
 * be of the form (indexkey OP comparisonval) or (comparisonval OP indexkey).
 * The indexkey is a Var or expression referencing column(s) of the index's
 * base table.  The comparisonval might be any expression, but it won't use
 * any columns of the base table.
 *
 * indexqual has the same form, but the expressions have been commuted if
 * necessary to put the indexkeys on the left, and the indexkeys are replaced
 * by Var nodes identifying the index columns (varattno is the index column
 * position, not the base table's column, even though varno is for the base
 * table).  This is a bit hokey ... would be cleaner to use a special-purpose
 * node type that could not be mistaken for a regular Var.  But it will do
 * for now.
 * ----------------
 */
typedef struct IndexScan
{
    Scan        scan;
    Oid         indexid;        /* OID of index to scan */
    List       *indexqual;      /* list of index quals (OpExprs) */
    List       *indexqualorig;  /* the same in original form */
    ScanDirection indexorderdir;    /* forward or backward or don't care */
} IndexScan;

```