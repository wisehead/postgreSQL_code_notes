#1.struct Unique

```cpp
/* ----------------
 *      unique node
 * ----------------
 */
typedef struct Unique
{
    Plan        plan;
    int         numCols;        /* number of columns to check for uniqueness */
    AttrNumber *uniqColIdx;     /* their indexes in the target list */
    Oid        *uniqOperators;  /* equality operators to compare with */
} Unique;


```