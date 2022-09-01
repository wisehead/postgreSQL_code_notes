#1.struct TransactionStateData

```cpp
/*
 *  transaction state structure
 */
typedef struct TransactionStateData
{
    TransactionId transactionId;    /* my XID, or Invalid if none */
    SubTransactionId subTransactionId;  /* my subxact ID */
    char       *name;           /* savepoint name, if any */
    int         savepointLevel; /* savepoint level */
    TransState  state;          /* low-level state */
    TBlockState blockState;     /* high-level state */
    int         nestingLevel;   /* transaction nesting depth */
    int         gucNestLevel;   /* GUC context nesting depth */
    MemoryContext curTransactionContext;        /* my xact-lifetime context */
    ResourceOwner curTransactionOwner;  /* my query resources */
    TransactionId *childXids;   /* subcommitted child XIDs, in XID order */
    int         nChildXids;     /* # of subcommitted child XIDs */
    int         maxChildXids;   /* allocated size of childXids[] */
    Oid         prevUser;       /* previous CurrentUserId setting */
    int         prevSecContext; /* previous SecurityRestrictionContext */
    bool        prevXactReadOnly;       /* entry-time xact r/o state */
    struct TransactionStateData *parent;        /* back link to parent */
} TransactionStateData;

typedef TransactionStateData *TransactionState;

```