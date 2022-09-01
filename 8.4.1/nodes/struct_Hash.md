#1.struct Hash

```cpp
/* ----------------
 *      hash build node
 *
 * If the executor is supposed to try to apply skew join optimization, then
 * skewTable/skewColumn identify the outer relation's join key column, from
 * which the relevant MCV statistics can be fetched.  Also, its type
 * information is provided to save a lookup.
 * ----------------
 */
typedef struct Hash
{
    Plan        plan;
    Oid         skewTable;      /* outer join key's table OID, or InvalidOid */
    AttrNumber  skewColumn;     /* outer join key's column #, or zero */
    Oid         skewColType;    /* datatype of the outer key column */
    int32       skewColTypmod;  /* typmod of the outer key column */
    /* all other info is in the parent HashJoin node */
} Hash;

```