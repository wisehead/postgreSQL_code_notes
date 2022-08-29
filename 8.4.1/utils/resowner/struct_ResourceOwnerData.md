#1.struct ResourceOwnerData

```cpp
/*
 * ResourceOwner objects look like this
 */
typedef struct ResourceOwnerData
{
    ResourceOwner parent;       /* NULL if no parent (toplevel owner) */
    ResourceOwner firstchild;   /* head of linked list of children */
    ResourceOwner nextchild;    /* next child of same parent */
    const char *name;           /* name (just for debugging) */

    /* We have built-in support for remembering owned buffers */
    int         nbuffers;       /* number of owned buffer pins */
    Buffer     *buffers;        /* dynamically allocated array */
    int         maxbuffers;     /* currently allocated array size */

    /* We have built-in support for remembering catcache references */
    int         ncatrefs;       /* number of owned catcache pins */
    HeapTuple  *catrefs;        /* dynamically allocated array */
    int         maxcatrefs;     /* currently allocated array size */

    int         ncatlistrefs;   /* number of owned catcache-list pins */
    CatCList  **catlistrefs;    /* dynamically allocated array */
    int         maxcatlistrefs; /* currently allocated array size */

    /* We have built-in support for remembering relcache references */
    int         nrelrefs;       /* number of owned relcache pins */
    Relation   *relrefs;        /* dynamically allocated array */
    int         maxrelrefs;     /* currently allocated array size */

    /* We have built-in support for remembering plancache references */
    int         nplanrefs;      /* number of owned plancache pins */
    CachedPlan **planrefs;      /* dynamically allocated array */
    int         maxplanrefs;    /* currently allocated array size */

    /* We have built-in support for remembering tupdesc references */
    int         ntupdescs;      /* number of owned tupdesc references */
    TupleDesc  *tupdescs;       /* dynamically allocated array */
    int         maxtupdescs;    /* currently allocated array size */

    /* We have built-in support for remembering snapshot references */
    int         nsnapshots;     /* number of owned snapshot references */
    Snapshot   *snapshots;      /* dynamically allocated array */
    int         maxsnapshots;   /* currently allocated array size */

    /* We have built-in support for remembering open temporary files */
    int         nfiles;         /* number of owned temporary files */
    File       *files;          /* dynamically allocated array */
    int         maxfiles;       /* currently allocated array size */
} ResourceOwnerData;    

```