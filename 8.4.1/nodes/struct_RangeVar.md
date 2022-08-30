#1.struct RangeVar

```cpp
/*
 * RangeVar - range variable, used in FROM clauses
 *
 * Also used to represent table names in utility statements; there, the alias
 * field is not used, and inhOpt shows whether to apply the operation
 * recursively to child tables.  In some contexts it is also useful to carry
 * a TEMP table indication here.
 */
typedef struct RangeVar
{
    NodeTag     type;
    char       *catalogname;    /* the catalog (database) name, or NULL */
    char       *schemaname;     /* the schema name, or NULL */
    char       *relname;        /* the relation/sequence name */
    InhOption   inhOpt;         /* expand rel by inheritance? recursively act
                                 * on children? */
    bool        istemp;         /* is this a temp relation/sequence? */
    Alias      *alias;          /* table alias & optional column aliases */
    int         location;       /* token location, or -1 if unknown */
} RangeVar;

```