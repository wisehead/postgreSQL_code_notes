#1.struct dsm_segment_detach_callback

```cpp
/* Backend-local tracking for on-detach callbacks. */
typedef struct dsm_segment_detach_callback
{
	on_dsm_detach_callback function;
	Datum		arg;
	slist_node	node;
} dsm_segment_detach_callback;
```