#1.struct XLogRecData

```cpp
/*
 * The functions in xloginsert.c construct a chain of XLogRecData structs
 * to represent the final WAL record.
 */
typedef struct XLogRecData
{
	struct XLogRecData *next;	/* next struct in chain, or NULL */
	char	   *data;			/* start of rmgr data to include */
	uint32		len;			/* length of rmgr data to include */
} XLogRecData;

```