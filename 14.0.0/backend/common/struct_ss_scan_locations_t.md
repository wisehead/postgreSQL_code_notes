#1.struct ss_scan_locations_t

```cpp
typedef struct ss_scan_locations_t
{
	ss_lru_item_t *head;
	ss_lru_item_t *tail;
	ss_lru_item_t items[FLEXIBLE_ARRAY_MEMBER]; /* SYNC_SCAN_NELEM items */
} ss_scan_locations_t;

#define SizeOfScanLocations(N) \
	(offsetof(ss_scan_locations_t, items) + (N) * sizeof(ss_lru_item_t))

/* Pointer to struct in shared memory */
static ss_scan_locations_t *scan_locations;
```