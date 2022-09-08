#1.struct LogicalTape

```cpp
/*
 * This data structure represents a single "logical tape" within the set
 * of logical tapes stored in the same file.
 *
 * While writing, we hold the current partially-written data block in the
 * buffer.  While reading, we can hold multiple blocks in the buffer.  Note
 * that we don't retain the trailers of a block when it's read into the
 * buffer.  The buffer therefore contains one large contiguous chunk of data
 * from the tape.
 */
typedef struct LogicalTape
{
	bool		writing;		/* T while in write phase */
	bool		frozen;			/* T if blocks should not be freed when read */
	bool		dirty;			/* does buffer need to be written? */

	/*
	 * Block numbers of the first, current, and next block of the tape.
	 *
	 * The "current" block number is only valid when writing, or reading from
	 * a frozen tape.  (When reading from an unfrozen tape, we use a larger
	 * read buffer that holds multiple blocks, so the "current" block is
	 * ambiguous.)
	 *
	 * When concatenation of worker tape BufFiles is performed, an offset to
	 * the first block in the unified BufFile space is applied during reads.
	 */
	long		firstBlockNumber;
	long		curBlockNumber;
	long		nextBlockNumber;
	long		offsetBlockNumber;

	/*
	 * Buffer for current data block(s).
	 */
	char	   *buffer;			/* physical buffer (separately palloc'd) */
	int			buffer_size;	/* allocated size of the buffer */
	int			max_size;		/* highest useful, safe buffer_size */
	int			pos;			/* next read/write position in buffer */
	int			nbytes;			/* total # of valid bytes in buffer */

	/*
	 * Preallocated block numbers are held in an array sorted in descending
	 * order; blocks are consumed from the end of the array (lowest block
	 * numbers first).
	 */
	long	   *prealloc;
	int			nprealloc;		/* number of elements in list */
	int			prealloc_size;	/* number of elements list can hold */
} LogicalTape;
```