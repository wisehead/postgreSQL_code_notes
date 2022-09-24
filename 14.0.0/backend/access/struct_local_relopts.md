#1.struct local_relopts

```cpp
/* Structure to hold local reloption data for build_local_reloptions() */
typedef struct local_relopts
{
	List	   *options;		/* list of local_relopt definitions */
	List	   *validators;		/* list of relopts_validator callbacks */
	Size		relopt_struct_size; /* size of parsed bytea structure */
} local_relopts;

```