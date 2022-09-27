#1.struct TransactionStateData

```cpp
/*
 *	transaction state structure
 */
typedef struct TransactionStateData
{
	FullTransactionId fullTransactionId;	/* my FullTransactionId */
	SubTransactionId subTransactionId;	/* my subxact ID */
	char	   *name;			/* savepoint name, if any */
	int			savepointLevel; /* savepoint level */
	TransState	state;			/* low-level state */
	TBlockState blockState;		/* high-level state */
	int			nestingLevel;	/* transaction nesting depth */
	int			gucNestLevel;	/* GUC context nesting depth */
	MemoryContext curTransactionContext;	/* my xact-lifetime context */
	ResourceOwner curTransactionOwner;	/* my query resources */
	TransactionId *childXids;	/* subcommitted child XIDs, in XID order */
	int			nChildXids;		/* # of subcommitted child XIDs */
	int			maxChildXids;	/* allocated size of childXids[] */
	Oid			prevUser;		/* previous CurrentUserId setting */
	int			prevSecContext; /* previous SecurityRestrictionContext */
	bool		prevXactReadOnly;	/* entry-time xact r/o state */
	bool		startedInRecovery;	/* did we start in recovery? */
	bool		didLogXid;		/* has xid been included in WAL record? */
	int			parallelModeLevel;	/* Enter/ExitParallelMode counter */
	bool		chain;			/* start a new block after this one */
	bool		assigned;		/* assigned to top-level XID */
	struct TransactionStateData *parent;	/* back link to parent */
} TransactionStateData;

typedef TransactionStateData *TransactionState;
```