#1.struct ExecRowMark

```cpp
/*
 * ExecRowMark -
 *	   runtime representation of FOR [KEY] UPDATE/SHARE clauses
 *
 * When doing UPDATE, DELETE, or SELECT FOR [KEY] UPDATE/SHARE, we will have an
 * ExecRowMark for each non-target relation in the query (except inheritance
 * parent RTEs, which can be ignored at runtime).  Virtual relations such as
 * subqueries-in-FROM will have an ExecRowMark with relation == NULL.  See
 * PlanRowMark for details about most of the fields.  In addition to fields
 * directly derived from PlanRowMark, we store an activity flag (to denote
 * inactive children of inheritance trees), curCtid, which is used by the
 * WHERE CURRENT OF code, and ermExtra, which is available for use by the plan
 * node that sources the relation (e.g., for a foreign table the FDW can use
 * ermExtra to hold information).
 *
 * EState->es_rowmarks is an array of these structs, indexed by RT index,
 * with NULLs for irrelevant RT indexes.  es_rowmarks itself is NULL if
 * there are no rowmarks.
 */
typedef struct ExecRowMark
{
	Relation	relation;		/* opened and suitably locked relation */
	Oid			relid;			/* its OID (or InvalidOid, if subquery) */
	Index		rti;			/* its range table index */
	Index		prti;			/* parent range table index, if child */
	Index		rowmarkId;		/* unique identifier for resjunk columns */
	RowMarkType markType;		/* see enum in nodes/plannodes.h */
	LockClauseStrength strength;	/* LockingClause's strength, or LCS_NONE */
	LockWaitPolicy waitPolicy;	/* NOWAIT and SKIP LOCKED */
	bool		ermActive;		/* is this mark relevant for current tuple? */
	ItemPointerData curCtid;	/* ctid of currently locked tuple, if any */
	void	   *ermExtra;		/* available for use by relation source node */
} ExecRowMark;
```