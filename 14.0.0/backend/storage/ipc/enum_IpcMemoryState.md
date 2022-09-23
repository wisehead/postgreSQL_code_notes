#1.enum IpcMemoryState

```cpp
/*
 * How does a given IpcMemoryId relate to this PostgreSQL process?
 *
 * One could recycle unattached segments of different data directories if we
 * distinguished that case from other SHMSTATE_FOREIGN cases.  Doing so would
 * cause us to visit less of the key space, making us less likely to detect a
 * SHMSTATE_ATTACHED key.  It would also complicate the concurrency analysis,
 * in that postmasters of different data directories could simultaneously
 * attempt to recycle a given key.  We'll waste keys longer in some cases, but
 * avoiding the problems of the alternative justifies that loss.
 */
typedef enum
{
	SHMSTATE_ANALYSIS_FAILURE,	/* unexpected failure to analyze the ID */
	SHMSTATE_ATTACHED,			/* pertinent to DataDir, has attached PIDs */
	SHMSTATE_ENOENT,			/* no segment of that ID */
	SHMSTATE_FOREIGN,			/* exists, but not pertinent to DataDir */
	SHMSTATE_UNATTACHED			/* pertinent to DataDir, no attached PIDs */
} IpcMemoryState;
```