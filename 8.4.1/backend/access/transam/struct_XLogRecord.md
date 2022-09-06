#1.struct XLogRecord

```cpp
/*
 * The overall layout of an XLOG record is:
 *      Fixed-size header (XLogRecord struct)
 *      rmgr-specific data
 *      BkpBlock
 *      backup block data
 *      BkpBlock
 *      backup block data
 *      ...
 *
 * where there can be zero to three backup blocks (as signaled by xl_info flag
 * bits).  XLogRecord structs always start on MAXALIGN boundaries in the WAL
 * files, and we round up SizeOfXLogRecord so that the rmgr data is also
 * guaranteed to begin on a MAXALIGN boundary.  However, no padding is added
 * to align BkpBlock structs or backup block data.
 *
 * NOTE: xl_len counts only the rmgr data, not the XLogRecord header,
 * and also not any backup blocks.  xl_tot_len counts everything.  Neither
 * length field is rounded up to an alignment boundary.
 */
typedef struct XLogRecord
{
    pg_crc32    xl_crc;         /* CRC for this record */
    XLogRecPtr  xl_prev;        /* ptr to previous record in log */
    TransactionId xl_xid;       /* xact id */
    uint32      xl_tot_len;     /* total len of entire record */
    uint32      xl_len;         /* total len of rmgr data */
    uint8       xl_info;        /* flag bits, see below */
    RmgrId      xl_rmid;        /* resource manager for this record */

    /* Depending on MAXALIGN, there are either 2 or 6 wasted bytes here */

    /* ACTUAL LOG DATA FOLLOWS AT END OF STRUCT */

} XLogRecord;

```