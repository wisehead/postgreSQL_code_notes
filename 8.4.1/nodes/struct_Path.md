#1.struct Path

```cpp

/*
 * Type "Path" is used as-is for sequential-scan paths, as well as some other
 * simple plan types that we don't need any extra information in the path for.
 * For other path types it is the first component of a larger struct.
 *
 * Note: "pathtype" is the NodeTag of the Plan node we could build from this
 * Path.  It is partially redundant with the Path's NodeTag, but allows us
 * to use the same Path type for multiple Plan types where there is no need
 * to distinguish the Plan type during path processing.
 */

typedef struct Path
{
    NodeTag     type;

    NodeTag     pathtype;       /* tag identifying scan/join method */

    RelOptInfo *parent;         /* the relation this path can build */

    /* estimated execution costs for path (see costsize.c for more info) */
    Cost        startup_cost;   /* cost expended before fetching any tuples */
    Cost        total_cost;     /* total cost (assuming all tuples fetched) */

    List       *pathkeys;       /* sort ordering of path's output */
    /* pathkeys is a List of PathKey nodes; see above */
} Path;
```