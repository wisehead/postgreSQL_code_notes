#1.struct relopt_gen

```cpp
/* generic struct to hold shared data */
typedef struct relopt_gen
{
	const char *name;			/* must be first (used as list termination
								 * marker) */
	const char *desc;
	bits32		kinds;
	LOCKMODE	lockmode;
	int			namelen;
	relopt_type type;
} relopt_gen;
```