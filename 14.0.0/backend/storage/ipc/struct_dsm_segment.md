#1.struct dsm_segment

```cpp
/* Backend-local state for a dynamic shared memory segment. */
struct dsm_segment
{
	dlist_node	node;			/* List link in dsm_segment_list. */
	ResourceOwner resowner;		/* Resource owner. */
	dsm_handle	handle;			/* Segment name. */
	uint32		control_slot;	/* Slot in control segment. */
	void	   *impl_private;	/* Implementation-specific private data. */
	void	   *mapped_address; /* Mapping address, or NULL if unmapped. */
	Size		mapped_size;	/* Size of our mapping. */
	slist_head	on_detach;		/* On-detach callbacks. */
};
```