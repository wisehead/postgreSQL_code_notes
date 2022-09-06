#1.struct FormData_pg_proc

```cpp

/* ----------------
 *		pg_proc definition.  cpp turns this into
 *		typedef struct FormData_pg_proc
 * ----------------
 */
CATALOG(pg_proc,1255,ProcedureRelationId) BKI_BOOTSTRAP BKI_ROWTYPE_OID(81,ProcedureRelation_Rowtype_Id) BKI_SCHEMA_MACRO
{
	Oid			oid;			/* oid */

	/* procedure name */
	NameData	proname;

	/* OID of namespace containing this proc */
	Oid			pronamespace BKI_DEFAULT(pg_catalog) BKI_LOOKUP(pg_namespace);

	/* procedure owner */
	Oid			proowner BKI_DEFAULT(POSTGRES) BKI_LOOKUP(pg_authid);

	/* OID of pg_language entry */
	Oid			prolang BKI_DEFAULT(internal) BKI_LOOKUP(pg_language);

	/* estimated execution cost */
	float4		procost BKI_DEFAULT(1);

	/* estimated # of rows out (if proretset) */
	float4		prorows BKI_DEFAULT(0);

	/* element type of variadic array, or 0 if not variadic */
	Oid			provariadic BKI_DEFAULT(0) BKI_LOOKUP_OPT(pg_type);

	/* planner support function for this function, or 0 if none */
	regproc		prosupport BKI_DEFAULT(0) BKI_LOOKUP_OPT(pg_proc);

	/* see PROKIND_ categories below */
	char		prokind BKI_DEFAULT(f);

	/* security definer */
	bool		prosecdef BKI_DEFAULT(f);

	/* is it a leak-proof function? */
	bool		proleakproof BKI_DEFAULT(f);

	/* strict with respect to NULLs? */
	bool		proisstrict BKI_DEFAULT(t);

	/* returns a set? */
	bool		proretset BKI_DEFAULT(f);

	/* see PROVOLATILE_ categories below */
	char		provolatile BKI_DEFAULT(i);

	/* see PROPARALLEL_ categories below */
	char		proparallel BKI_DEFAULT(s);

	/* number of arguments */
	/* Note: need not be given in pg_proc.dat; genbki.pl will compute it */
	int16		pronargs;

	/* number of arguments with defaults */
	int16		pronargdefaults BKI_DEFAULT(0);

	/* OID of result type */
	Oid			prorettype BKI_LOOKUP(pg_type);

	/*
	 * variable-length fields start here, but we allow direct access to
	 * proargtypes
	 */

	/* parameter types (excludes OUT params) */
	oidvector	proargtypes BKI_LOOKUP(pg_type) BKI_FORCE_NOT_NULL;

#ifdef CATALOG_VARLEN

	/* all param types (NULL if IN only) */
	Oid			proallargtypes[1] BKI_DEFAULT(_null_) BKI_LOOKUP(pg_type);

	/* parameter modes (NULL if IN only) */
	char		proargmodes[1] BKI_DEFAULT(_null_);

	/* parameter names (NULL if no names) */
	text		proargnames[1] BKI_DEFAULT(_null_);

	/* list of expression trees for argument defaults (NULL if none) */
	pg_node_tree proargdefaults BKI_DEFAULT(_null_);

	/* types for which to apply transforms */
	Oid			protrftypes[1] BKI_DEFAULT(_null_) BKI_LOOKUP(pg_type);

	/* procedure source text */
	text		prosrc BKI_FORCE_NOT_NULL;

	/* secondary procedure info (can be NULL) */
	text		probin BKI_DEFAULT(_null_);

	/* pre-parsed SQL function body */
	pg_node_tree prosqlbody BKI_DEFAULT(_null_);

	/* procedure-local GUC settings */
	text		proconfig[1] BKI_DEFAULT(_null_);

	/* access permissions */
	aclitem		proacl[1] BKI_DEFAULT(_null_);
#endif
} FormData_pg_proc;


```

#2.CATALOG(pg_proc,1255,ProcedureRelationId)

```cpp
/* Introduces a catalog's structure definition */
#define CATALOG(name,oid,oidmacro)	typedef struct CppConcat(FormData_,name)

```

#3.BKI_ROWTYPE_OID(81,ProcedureRelation_Rowtype_Id) 

```cpp

```

#4.

```cpp
/* Options that may appear after CATALOG (on the same line) */
#define BKI_BOOTSTRAP
#define BKI_SHARED_RELATION
#define BKI_ROWTYPE_OID(oid,oidmacro)
#define BKI_SCHEMA_MACRO
```