#1.struct ResTarget

```cpp
/*
 * ResTarget -
 *    result target (used in target list of pre-transformed parse trees)
 *
 * In a SELECT target list, 'name' is the column label from an
 * 'AS ColumnLabel' clause, or NULL if there was none, and 'val' is the
 * value expression itself.  The 'indirection' field is not used.
 *
 * INSERT uses ResTarget in its target-column-names list.  Here, 'name' is
 * the name of the destination column, 'indirection' stores any subscripts
 * attached to the destination, and 'val' is not used.
 *
 * In an UPDATE target list, 'name' is the name of the destination column,
 * 'indirection' stores any subscripts attached to the destination, and
 * 'val' is the expression to assign.
 *
 * See A_Indirection for more info about what can appear in 'indirection'.
 */
typedef struct ResTarget
{
    NodeTag     type;
    char       *name;           /* column name or NULL */
    List       *indirection;    /* subscripts, field names, and '*', or NIL */
    Node       *val;            /* the value expression to compute or assign */
    int         location;       /* token location, or -1 if unknown */
} ResTarget;

```