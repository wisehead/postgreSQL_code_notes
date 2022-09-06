#1.struct Sort

```cpp
/* ----------------
 *      sort node
 * ----------------
 */
typedef struct Sort
{
    Plan        plan;
    int         numCols;        /* number of sort-key columns */
    AttrNumber *sortColIdx;     /* their indexes in the target list */
    Oid        *sortOperators;  /* OIDs of operators to sort them by */
    bool       *nullsFirst;     /* NULLS FIRST/LAST directions */
} Sort;
```