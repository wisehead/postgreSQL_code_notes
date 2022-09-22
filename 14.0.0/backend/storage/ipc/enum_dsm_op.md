#1.enum dsm_op

```cpp
/* All the shared-memory operations we know about. */
typedef enum
{
	DSM_OP_CREATE,
	DSM_OP_ATTACH,
	DSM_OP_DETACH,
	DSM_OP_DESTROY
} dsm_op;
```