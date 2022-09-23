#1.struct FormData_pg_class

```cpp
/* ----------------
 *		pg_class definition.  cpp turns this into
 *		typedef struct FormData_pg_class
 *
 * Note that the BKI_DEFAULT values below are only used for rows describing
 * BKI_BOOTSTRAP catalogs, since only those rows appear in pg_class.dat.
 * ----------------
 */
CATALOG(pg_class,1259,RelationRelationId) BKI_BOOTSTRAP BKI_ROWTYPE_OID(83,RelationRelation_Rowtype_Id) BKI_SCHEMA_MACRO
{
	/* oid */
	Oid			oid;

	/* class name */
	NameData	relname;

	/* OID of namespace containing this class */
	Oid			relnamespace BKI_DEFAULT(pg_catalog) BKI_LOOKUP(pg_namespace);

	/* OID of entry in pg_type for relation's implicit row type, if any */
	Oid			reltype BKI_LOOKUP_OPT(pg_type);

	/* OID of entry in pg_type for underlying composite type, if any */
	Oid			reloftype BKI_DEFAULT(0) BKI_LOOKUP_OPT(pg_type);

	/* class owner */
	Oid			relowner BKI_DEFAULT(POSTGRES) BKI_LOOKUP(pg_authid);

	/* access method; 0 if not a table / index */
	Oid			relam BKI_DEFAULT(heap) BKI_LOOKUP_OPT(pg_am);

	/* identifier of physical storage file */
	/* relfilenode == 0 means it is a "mapped" relation, see relmapper.c */
	Oid			relfilenode BKI_DEFAULT(0);

	/* identifier of table space for relation (0 means default for database) */
	Oid			reltablespace BKI_DEFAULT(0) BKI_LOOKUP_OPT(pg_tablespace);

	/* # of blocks (not always up-to-date) */
	int32		relpages BKI_DEFAULT(0);

	/* # of tuples (not always up-to-date; -1 means "unknown") */
	float4		reltuples BKI_DEFAULT(-1);

	/* # of all-visible blocks (not always up-to-date) */
	int32		relallvisible BKI_DEFAULT(0);

	/* OID of toast table; 0 if none */
	Oid			reltoastrelid BKI_DEFAULT(0) BKI_LOOKUP_OPT(pg_class);

	/* T if has (or has had) any indexes */
	bool		relhasindex BKI_DEFAULT(f);

	/* T if shared across databases */
	bool		relisshared BKI_DEFAULT(f);

	/* see RELPERSISTENCE_xxx constants below */
	char		relpersistence BKI_DEFAULT(p);

	/* see RELKIND_xxx constants below */
	char		relkind BKI_DEFAULT(r);

	/* number of user attributes */
	int16		relnatts BKI_DEFAULT(0);	/* genbki.pl will fill this in */

	/*
	 * Class pg_attribute must contain exactly "relnatts" user attributes
	 * (with attnums ranging from 1 to relnatts) for this class.  It may also
	 * contain entries with negative attnums for system attributes.
	 */

	/* # of CHECK constraints for class */
	int16		relchecks BKI_DEFAULT(0);

	/* has (or has had) any rules */
	bool		relhasrules BKI_DEFAULT(f);

	/* has (or has had) any TRIGGERs */
	bool		relhastriggers BKI_DEFAULT(f);

	/* has (or has had) child tables or indexes */
	bool		relhassubclass BKI_DEFAULT(f);

	/* row security is enabled or not */
	bool		relrowsecurity BKI_DEFAULT(f);

	/* row security forced for owners or not */
	bool		relforcerowsecurity BKI_DEFAULT(f);

	/* matview currently holds query results */
	bool		relispopulated BKI_DEFAULT(t);

	/* see REPLICA_IDENTITY_xxx constants */
	char		relreplident BKI_DEFAULT(n);

	/* is relation a partition? */
	bool		relispartition BKI_DEFAULT(f);

	/* link to original rel during table rewrite; otherwise 0 */
	Oid			relrewrite BKI_DEFAULT(0) BKI_LOOKUP_OPT(pg_class);

	/* all Xids < this are frozen in this rel */
	TransactionId relfrozenxid BKI_DEFAULT(3);	/* FirstNormalTransactionId */

	/* all multixacts in this rel are >= this; it is really a MultiXactId */
	TransactionId relminmxid BKI_DEFAULT(1);	/* FirstMultiXactId */

#ifdef CATALOG_VARLEN			/* variable-length fields start here */
	/* NOTE: These fields are not present in a relcache entry's rd_rel field. */
	/* access permissions */
	aclitem		relacl[1] BKI_DEFAULT(_null_);

	/* access-method-specific options */
	text		reloptions[1] BKI_DEFAULT(_null_);

	/* partition bound node tree */
	pg_node_tree relpartbound BKI_DEFAULT(_null_);
#endif
} FormData_pg_class;


```