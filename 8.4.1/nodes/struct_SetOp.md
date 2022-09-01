#1.struct SetOp

```cpp

typedef struct SetOp
{
    Plan        plan;
    SetOpCmd    cmd;            /* what to do */
    SetOpStrategy strategy;     /* how to do it */
    int         numCols;        /* number of columns to check for
                                 * duplicate-ness */
    AttrNumber *dupColIdx;      /* their indexes in the target list */
    Oid        *dupOperators;   /* equality operators to compare with */
    AttrNumber  flagColIdx;     /* where is the flag column, if any */
    int         firstFlag;      /* flag value for first input relation */
    long        numGroups;      /* estimated number of groups in input */
} SetOp;

```