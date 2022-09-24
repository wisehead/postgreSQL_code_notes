#1.struct TupleConstr

```cpp
/* This structure contains constraints of a tuple */
typedef struct TupleConstr
{
	AttrDefault *defval;		/* array */
	ConstrCheck *check;			/* array */
	struct AttrMissing *missing;	/* missing attributes values, NULL if none */
	uint16		num_defval;
	uint16		num_check;
	bool		has_not_null;
	bool		has_generated_stored;
} TupleConstr;
```