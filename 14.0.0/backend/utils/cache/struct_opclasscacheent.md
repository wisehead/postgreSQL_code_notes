#1.struct opclasscacheent

```cpp
/*
 * Special cache for opclass-related information
 *
 * Note: only default support procs get cached, ie, those with
 * lefttype = righttype = opcintype.
 */
typedef struct opclasscacheent
{
	Oid			opclassoid;		/* lookup key: OID of opclass */
	bool		valid;			/* set true after successful fill-in */
	StrategyNumber numSupport;	/* max # of support procs (from pg_am) */
	Oid			opcfamily;		/* OID of opclass's family */
	Oid			opcintype;		/* OID of opclass's declared input type */
	RegProcedure *supportProcs; /* OIDs of support procedures */
} OpClassCacheEnt;

```