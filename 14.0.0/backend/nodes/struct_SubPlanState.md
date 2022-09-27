#1.struct SubPlanState

```cpp
/* ----------------
 *		SubPlanState node
 * ----------------
 */
typedef struct SubPlanState
{
	NodeTag		type;
	SubPlan    *subplan;		/* expression plan node */
	struct PlanState *planstate;	/* subselect plan's state tree */
	struct PlanState *parent;	/* parent plan node's state tree */
	ExprState  *testexpr;		/* state of combining expression */
	List	   *args;			/* states of argument expression(s) */
	HeapTuple	curTuple;		/* copy of most recent tuple from subplan */
	Datum		curArray;		/* most recent array from ARRAY() subplan */
	/* these are used when hashing the subselect's output: */
	TupleDesc	descRight;		/* subselect desc after projection */
	ProjectionInfo *projLeft;	/* for projecting lefthand exprs */
	ProjectionInfo *projRight;	/* for projecting subselect output */
	TupleHashTable hashtable;	/* hash table for no-nulls subselect rows */
	TupleHashTable hashnulls;	/* hash table for rows with null(s) */
	bool		havehashrows;	/* true if hashtable is not empty */
	bool		havenullrows;	/* true if hashnulls is not empty */
	MemoryContext hashtablecxt; /* memory context containing hash tables */
	MemoryContext hashtempcxt;	/* temp memory context for hash tables */
	ExprContext *innerecontext; /* econtext for computing inner tuples */
	int			numCols;		/* number of columns being hashed */
	/* each of the remaining fields is an array of length numCols: */
	AttrNumber *keyColIdx;		/* control data for hash tables */
	Oid		   *tab_eq_funcoids;	/* equality func oids for table
									 * datatype(s) */
	Oid		   *tab_collations; /* collations for hash and comparison */
	FmgrInfo   *tab_hash_funcs; /* hash functions for table datatype(s) */
	FmgrInfo   *tab_eq_funcs;	/* equality functions for table datatype(s) */
	FmgrInfo   *lhs_hash_funcs; /* hash functions for lefthand datatype(s) */
	FmgrInfo   *cur_eq_funcs;	/* equality functions for LHS vs. table */
	ExprState  *cur_eq_comp;	/* equality comparator for LHS vs. table */
} SubPlanState;
```