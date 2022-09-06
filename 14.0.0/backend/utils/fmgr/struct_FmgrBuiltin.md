#1.struct FmgrBuiltin

```cpp
/*
 * This table stores info about all the built-in functions (ie, functions
 * that are compiled into the Postgres executable).
 */

typedef struct
{
	Oid			foid;			/* OID of the function */
	short		nargs;			/* 0..FUNC_MAX_ARGS, or -1 if variable count */
	bool		strict;			/* T if function is "strict" */
	bool		retset;			/* T if function returns a set */
	const char *funcName;		/* C name of the function */
	PGFunction	func;			/* pointer to compiled function */
} FmgrBuiltin;
```