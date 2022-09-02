#1. struct IndexInfo

```cpp
/* ----------------
 *    IndexInfo information
 *
 *      this struct holds the information needed to construct new index
 *      entries for a particular index.  Used for both index_build and
 *      retail creation of index entries.
 *
 *      NumIndexAttrs       total number of columns in this index
 *      NumIndexKeyAttrs    number of key columns in index
 *      IndexAttrNumbers    underlying-rel attribute numbers used as keys
 *                          (zeroes indicate expressions). It also contains
 *                          info about included columns.
 *      Expressions         expr trees for expression entries, or NIL if none
 *      ExpressionsState    exec state for expressions, or NIL if none
 *      Predicate           partial-index predicate, or NIL if none
 *      PredicateState      exec state for predicate, or NIL if none
 *      ExclusionOps        Per-column exclusion operators, or NULL if none
 *      ExclusionProcs      Underlying function OIDs for ExclusionOps
 *      ExclusionStrats     Opclass strategy numbers for ExclusionOps
 *      UniqueOps           These are like Exclusion*, but for unique indexes
 *      UniqueProcs
 *      UniqueStrats
 *      Unique              is it a unique index?
 *      OpclassOptions      opclass-specific options, or NULL if none
 *      ReadyForInserts     is it valid for inserts?
 *      Concurrent          are we doing a concurrent index build?
 *      BrokenHotChain      did we detect any broken HOT chains?
 *      ParallelWorkers     # of workers requested (excludes leader)
 *      Am                  Oid of index AM
 *      AmCache             private cache area for index AM
 *      Context             memory context holding this IndexInfo
 *
 * ii_Concurrent, ii_BrokenHotChain, and ii_ParallelWorkers are used only
 * during index build; they're conventionally zeroed otherwise.
 * ----------------
 */
typedef struct IndexInfo
{
    NodeTag     type;
    int         ii_NumIndexAttrs;   /* total number of columns in index */
    int         ii_NumIndexKeyAttrs;    /* number of key columns in index */
    AttrNumber  ii_IndexAttrNumbers[INDEX_MAX_KEYS];
    List       *ii_Expressions; /* list of Expr */
    List       *ii_ExpressionsState;    /* list of ExprState */
    List       *ii_Predicate;   /* list of Expr */
    ExprState  *ii_PredicateState;
    Oid        *ii_ExclusionOps;    /* array with one entry per column */
    Oid        *ii_ExclusionProcs;  /* array with one entry per column */
    uint16     *ii_ExclusionStrats; /* array with one entry per column */
    Oid        *ii_UniqueOps;   /* array with one entry per column */
    Oid        *ii_UniqueProcs; /* array with one entry per column */
    uint16     *ii_UniqueStrats;    /* array with one entry per column */
    Datum      *ii_OpclassOptions;  /* array with one entry per column */
    bool        ii_Unique;
    bool        ii_ReadyForInserts;
    bool        ii_Concurrent;
    bool        ii_BrokenHotChain;
    int         ii_ParallelWorkers;
    Oid         ii_Am;
    void       *ii_AmCache;
    MemoryContext ii_Context;
} IndexInfo;
```