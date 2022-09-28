#1.struct IndexRuntimeKeyInfo

```cpp
/*
 * These structs store information about index quals that don't have simple
 * constant right-hand sides.  See comments for ExecIndexBuildScanKeys()
 * for discussion.
 */
typedef struct
{
	struct ScanKeyData *scan_key;	/* scankey to put value into */
	ExprState  *key_expr;		/* expr to evaluate to get value */
	bool		key_toastable;	/* is expr's result a toastable datatype? */
} IndexRuntimeKeyInfo;
```