#1.struct IndexArrayKeyInfo

```cpp
typedef struct
{
	struct ScanKeyData *scan_key;	/* scankey to put value into */
	ExprState  *array_expr;		/* expr to evaluate to get array value */
	int			next_elem;		/* next array element to use */
	int			num_elems;		/* number of elems in current array value */
	Datum	   *elem_values;	/* array of num_elems Datums */
	bool	   *elem_nulls;		/* array of num_elems is-null flags */
} IndexArrayKeyInfo;
```