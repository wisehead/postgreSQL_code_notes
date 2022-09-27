#1.struct ActiveSnapshotElt

```cpp
/*
 * Elements of the active snapshot stack.
 *
 * Each element here accounts for exactly one active_count on SnapshotData.
 *
 * NB: the code assumes that elements in this list are in non-increasing
 * order of as_level; also, the list must be NULL-terminated.
 */
typedef struct ActiveSnapshotElt
{
	Snapshot	as_snap;
	int			as_level;
	struct ActiveSnapshotElt *as_next;
} ActiveSnapshotElt;

```