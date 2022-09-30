#1.struct RangeTblFunction

```cpp
/*
 * RangeTblFunction -
 *	  RangeTblEntry subsidiary data for one function in a FUNCTION RTE.
 *
 * If the function had a column definition list (required for an
 * otherwise-unspecified RECORD result), funccolnames lists the names given
 * in the definition list, funccoltypes lists their declared column types,
 * funccoltypmods lists their typmods, funccolcollations their collations.
 * Otherwise, those fields are NIL.
 *
 * Notice we don't attempt to store info about the results of functions
 * returning named composite types, because those can change from time to
 * time.  We do however remember how many columns we thought the type had
 * (including dropped columns!), so that we can successfully ignore any
 * columns added after the query was parsed.
 */
typedef struct RangeTblFunction
{
	NodeTag		type;

	Node	   *funcexpr;		/* expression tree for func call */
	int			funccolcount;	/* number of columns it contributes to RTE */
	/* These fields record the contents of a column definition list, if any: */
	List	   *funccolnames;	/* column names (list of String) */
	List	   *funccoltypes;	/* OID list of column type OIDs */
	List	   *funccoltypmods; /* integer list of column typmods */
	List	   *funccolcollations;	/* OID list of column collation OIDs */
	/* This is set during planning for use by the executor: */
	Bitmapset  *funcparams;		/* PARAM_EXEC Param IDs affecting this func */
} RangeTblFunction;

```