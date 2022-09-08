#1.struct BufFile

```cpp
/*
 * This data structure represents a buffered file that consists of one or
 * more physical files (each accessed through a virtual file descriptor
 * managed by fd.c).
 */
struct BufFile
{
	int			numFiles;		/* number of physical files in set */
	/* all files except the last have length exactly MAX_PHYSICAL_FILESIZE */
	File	   *files;			/* palloc'd array with numFiles entries */

	bool		isInterXact;	/* keep open over transactions? */
	bool		dirty;			/* does buffer need to be written? */
	bool		readOnly;		/* has the file been set to read only? */

	SharedFileSet *fileset;		/* space for segment files if shared */
	const char *name;			/* name of this BufFile if shared */

	/*
	 * resowner is the ResourceOwner to use for underlying temp files.  (We
	 * don't need to remember the memory context we're using explicitly,
	 * because after creation we only repalloc our arrays larger.)
	 */
	ResourceOwner resowner;

	/*
	 * "current pos" is position of start of buffer within the logical file.
	 * Position as seen by user of BufFile is (curFile, curOffset + pos).
	 */
	int			curFile;		/* file index (0..n) part of current pos */
	off_t		curOffset;		/* offset part of current pos */
	int			pos;			/* next read/write position in buffer */
	int			nbytes;			/* total # of valid bytes in buffer */
	PGAlignedBlock buffer;
};
```