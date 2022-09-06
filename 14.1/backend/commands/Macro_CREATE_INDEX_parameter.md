#1.CREATE INDEX parameter

```cpp
/* Progress parameters for CREATE INDEX */
/* 3, 4 and 5 reserved for "waitfor" metrics */
#define PROGRESS_CREATEIDX_COMMAND				0
#define PROGRESS_CREATEIDX_INDEX_OID			6
#define PROGRESS_CREATEIDX_ACCESS_METHOD_OID	8
#define PROGRESS_CREATEIDX_PHASE				9	/* AM-agnostic phase # */
#define PROGRESS_CREATEIDX_SUBPHASE				10	/* phase # filled by AM */
#define PROGRESS_CREATEIDX_TUPLES_TOTAL			11
#define PROGRESS_CREATEIDX_TUPLES_DONE			12
#define PROGRESS_CREATEIDX_PARTITIONS_TOTAL		13
#define PROGRESS_CREATEIDX_PARTITIONS_DONE		14
/* 15 and 16 reserved for "block number" metrics */

```