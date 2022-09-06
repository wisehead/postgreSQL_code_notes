#1.struct WindowAgg

```cpp
/* ----------------
 *      window aggregate node
 * ----------------
 */
typedef struct WindowAgg
{
    Plan        plan;
    Index       winref;         /* ID referenced by window functions */
    int         partNumCols;    /* number of columns in partition clause */
    AttrNumber *partColIdx;     /* their indexes in the target list */
    Oid        *partOperators;  /* equality operators for partition columns */
    int         ordNumCols;     /* number of columns in ordering clause */
    AttrNumber *ordColIdx;      /* their indexes in the target list */
    Oid        *ordOperators;   /* equality operators for ordering columns */
    int         frameOptions;   /* frame_clause options, see WindowDef */
} WindowAgg;
```