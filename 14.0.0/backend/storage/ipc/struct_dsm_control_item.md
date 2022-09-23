#1.struct dsm_control_item

```cpp
/* Shared-memory state for a dynamic shared memory segment. */
typedef struct dsm_control_item
{
	dsm_handle	handle;
	uint32		refcnt;			/* 2+ = active, 1 = moribund, 0 = gone */
	size_t		first_page;
	size_t		npages;
	void	   *impl_private_pm_handle; /* only needed on Windows */
	bool		pinned;
} dsm_control_item;
```