#1.struct registered_buffer

```cpp
/*
 * For each block reference registered with XLogRegisterBuffer, we fill in
 * a registered_buffer struct.
 */
typedef struct
{
	bool		in_use;			/* is this slot in use? */
	uint8		flags;			/* REGBUF_* flags */
	RelFileNode rnode;			/* identifies the relation and block */
	ForkNumber	forkno;
	BlockNumber block;
	Page		page;			/* page content */
	uint32		rdata_len;		/* total length of data in rdata chain */
	XLogRecData *rdata_head;	/* head of the chain of data registered with
								 * this block */
	XLogRecData *rdata_tail;	/* last entry in the chain, or &rdata_head if
								 * empty */

	XLogRecData bkp_rdatas[2];	/* temporary rdatas used to hold references to
								 * backup block data in XLogRecordAssemble() */

	/* buffer to store a compressed version of backup block image */
	char		compressed_page[PGLZ_MAX_BLCKSZ];
} registered_buffer;
```
