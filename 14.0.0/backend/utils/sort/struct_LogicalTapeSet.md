#1.struct LogicalTapeSet

```cpp
/*
 * This data structure represents a set of related "logical tapes" sharing
 * space in a single underlying file.  (But that "file" may be multiple files
 * if needed to escape OS limits on file size; buffile.c handles that for us.)
 * The number of tapes is fixed at creation.
 */
struct LogicalTapeSet
{
	BufFile    *pfile;			/* underlying file for whole tape set */

	/*
	 * File size tracking.  nBlocksWritten is the size of the underlying file,
	 * in BLCKSZ blocks.  nBlocksAllocated is the number of blocks allocated
	 * by ltsReleaseBlock(), and it is always greater than or equal to
	 * nBlocksWritten.  Blocks between nBlocksAllocated and nBlocksWritten are
	 * blocks that have been allocated for a tape, but have not been written
	 * to the underlying file yet.  nHoleBlocks tracks the total number of
	 * blocks that are in unused holes between worker spaces following BufFile
	 * concatenation.
	 */
	long		nBlocksAllocated;	/* # of blocks allocated */
	long		nBlocksWritten; /* # of blocks used in underlying file */
	long		nHoleBlocks;	/* # of "hole" blocks left */

	/*
	 * We store the numbers of recycled-and-available blocks in freeBlocks[].
	 * When there are no such blocks, we extend the underlying file.
	 *
	 * If forgetFreeSpace is true then any freed blocks are simply forgotten
	 * rather than being remembered in freeBlocks[].  See notes for
	 * LogicalTapeSetForgetFreeSpace().
	 */
	bool		forgetFreeSpace;	/* are we remembering free blocks? */
	long	   *freeBlocks;		/* resizable array holding minheap */
	long		nFreeBlocks;	/* # of currently free blocks */
	Size		freeBlocksLen;	/* current allocated length of freeBlocks[] */
	bool		enable_prealloc;	/* preallocate write blocks? */

	/* The array of logical tapes. */
	int			nTapes;			/* # of logical tapes in set */
	LogicalTape *tapes;			/* has nTapes nentries */
};


```