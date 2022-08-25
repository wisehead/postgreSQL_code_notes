#1.struct RelationData

```cpp
/*
 * Here are the contents of a relation cache entry.
 */

typedef struct RelationData
{
    RelFileNode rd_node;        /* relation physical identifier */
    /* use "struct" here to avoid needing to include smgr.h: */
    struct SMgrRelationData *rd_smgr;   /* cached file handle, or NULL */
    BlockNumber rd_targblock;   /* current insertion target block, or
                                 * InvalidBlockNumber */
    int         rd_refcnt;      /* reference count */
    bool        rd_istemp;      /* rel is a temporary relation */
    bool        rd_islocaltemp; /* rel is a temp rel of this session */
    bool        rd_isnailed;    /* rel is nailed in cache */
    bool        rd_isvalid;     /* relcache entry is valid */
    char        rd_indexvalid;  /* state of rd_indexlist: 0 = not valid, 1 =
                                 * valid, 2 = temporarily forced */
    SubTransactionId rd_createSubid;    /* rel was created in current xact */
    SubTransactionId rd_newRelfilenodeSubid;    /* new relfilenode assigned in
                                                 * current xact */

    /*
     * rd_createSubid is the ID of the highest subtransaction the rel has
     * survived into; or zero if the rel was not created in the current top
     * transaction.  This should be relied on only for optimization purposes;
     * it is possible for new-ness to be "forgotten" (eg, after CLUSTER).
     * Likewise, rd_newRelfilenodeSubid is the ID of the highest
     * subtransaction the relfilenode change has survived into, or zero if not
     * changed in the current transaction (or we have forgotten changing it).
     */
    Form_pg_class rd_rel;       /* RELATION tuple */
    TupleDesc   rd_att;         /* tuple descriptor */
    Oid         rd_id;          /* relation's object id */
    List       *rd_indexlist;   /* list of OIDs of indexes on relation */
    Bitmapset  *rd_indexattr;   /* identifies columns used in indexes */
    Oid         rd_oidindex;    /* OID of unique index on OID, if any */
    LockInfoData rd_lockInfo;   /* lock mgr's info for locking relation */
    RuleLock   *rd_rules;       /* rewrite rules */
    MemoryContext rd_rulescxt;  /* private memory cxt for rd_rules, if any */
    TriggerDesc *trigdesc;      /* Trigger info, or NULL if rel has none */

    /*
     * rd_options is set whenever rd_rel is loaded into the relcache entry.
     * Note that you can NOT look into rd_rel for this data.  NULL means "use
     * defaults".
     */
    bytea      *rd_options;     /* parsed pg_class.reloptions */

    /* These are non-NULL only for an index relation: */
    Form_pg_index rd_index;     /* pg_index tuple describing this index */
    struct HeapTupleData *rd_indextuple;        /* all of pg_index tuple */
    /* "struct HeapTupleData *" avoids need to include htup.h here  */
    Form_pg_am  rd_am;          /* pg_am tuple for index's AM */

    /*
     * index access support info (used only for an index relation)
     *
     * Note: only default operators and support procs for each opclass are
     * cached, namely those with lefttype and righttype equal to the opclass's
     * opcintype.  The arrays are indexed by strategy or support number, which
     * is a sufficient identifier given that restriction.
     *
     * Note: rd_amcache is available for index AMs to cache private data about
     * an index.  This must be just a cache since it may get reset at any time
     * (in particular, it will get reset by a relcache inval message for the
     * index).  If used, it must point to a single memory chunk palloc'd in
     * rd_indexcxt.  A relcache reset will include freeing that chunk and
     * setting rd_amcache = NULL.
     */
    MemoryContext rd_indexcxt;  /* private memory cxt for this stuff */
    RelationAmInfo *rd_aminfo;  /* lookup info for funcs found in pg_am */
    Oid        *rd_opfamily;    /* OIDs of op families for each index col */
    Oid        *rd_opcintype;   /* OIDs of opclass declared input data types */
    Oid        *rd_operator;    /* OIDs of index operators */
    RegProcedure *rd_support;   /* OIDs of support procedures */
    FmgrInfo   *rd_supportinfo; /* lookup info for support procedures */
    int16      *rd_indoption;   /* per-column AM-specific flags */
    List       *rd_indexprs;    /* index expression trees, if any */
    List       *rd_indpred;     /* index predicate tree, if any */
    void       *rd_amcache;     /* available for use by index AM */

    /*
     * sizes of the free space and visibility map forks, or InvalidBlockNumber
     * if not known yet
     */
    BlockNumber rd_fsm_nblocks;
    BlockNumber rd_vm_nblocks;

    /* use "struct" here to avoid needing to include pgstat.h: */
    struct PgStat_TableStatus *pgstat_info;     /* statistics collection area */
} RelationData;    

```