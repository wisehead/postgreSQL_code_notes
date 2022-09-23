#1.struct pairingheap

```cpp
/*
 * A pairing heap.
 *
 * You can use pairingheap_allocate() to create a new palloc'd heap, or embed
 * this in a larger struct, set ph_compare and ph_arg directly and initialize
 * ph_root to NULL.
 */
typedef struct pairingheap
{
	pairingheap_comparator ph_compare;	/* comparison function */
	void	   *ph_arg;			/* opaque argument to ph_compare */
	pairingheap_node *ph_root;	/* current root of the heap */
} pairingheap;
```