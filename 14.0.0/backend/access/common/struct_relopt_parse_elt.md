#1.struct relopt_parse_elt

```cpp
/* This is the table datatype for build_reloptions() */
typedef struct
{
	const char *optname;		/* option's name */
	relopt_type opttype;		/* option's datatype */
	int			offset;			/* offset of field in result struct */
} relopt_parse_elt;
```