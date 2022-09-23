#1.struct dsm_control_header

```cpp
/* Layout of the dynamic shared memory control segment. */
typedef struct dsm_control_header
{
	uint32		magic;
	uint32		nitems;
	uint32		maxitems;
	dsm_control_item item[FLEXIBLE_ARRAY_MEMBER];
} dsm_control_header;
```