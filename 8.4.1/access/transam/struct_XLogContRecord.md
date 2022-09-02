#1.struct XLogContRecord

```cpp
/*
 * When there is not enough space on current page for whole record, we
 * continue on the next page with continuation record.  (However, the
 * XLogRecord header will never be split across pages; if there's less than
 * SizeOfXLogRecord space left at the end of a page, we just waste it.)
 *
 * Note that xl_rem_len includes backup-block data; that is, it tracks
 * xl_tot_len not xl_len in the initial header.  Also note that the
 * continuation data isn't necessarily aligned.
 */
typedef struct XLogContRecord
{
    uint32      xl_rem_len;     /* total len of remaining data for record */

    /* ACTUAL LOG DATA FOLLOWS AT END OF STRUCT */

} XLogContRecord;

```