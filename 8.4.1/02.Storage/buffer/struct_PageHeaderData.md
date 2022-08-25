#1.struct PageHeaderData

```cpp

/*
 * disk page organization
 *
 * space management information generic to any page
 *
 *      pd_lsn      - identifies xlog record for last change to this page.
 *      pd_tli      - ditto.
 *      pd_flags    - flag bits.
 *      pd_lower    - offset to start of free space.
 *      pd_upper    - offset to end of free space.
 *      pd_special  - offset to start of special space.
 *      pd_pagesize_version - size in bytes and page layout version number.
 *      pd_prune_xid - oldest XID among potentially prunable tuples on page.
 *
 * The LSN is used by the buffer manager to enforce the basic rule of WAL:
 * "thou shalt write xlog before data".  A dirty buffer cannot be dumped
 * to disk until xlog has been flushed at least as far as the page's LSN.
 * We also store the 16 least significant bits of the TLI for identification
 * purposes (it is not clear that this is actually necessary, but it seems
 * like a good idea).
 *
 * pd_prune_xid is a hint field that helps determine whether pruning will be
 * useful.  It is currently unused in index pages.
 *
 * The page version number and page size are packed together into a single
 * uint16 field.  This is for historical reasons: before PostgreSQL 7.3,
 * there was no concept of a page version number, and doing it this way
 * lets us pretend that pre-7.3 databases have page version number zero.
 * We constrain page sizes to be multiples of 256, leaving the low eight
 * bits available for a version number.
 *
 * Minimum possible page size is perhaps 64B to fit page header, opaque space
 * and a minimal tuple; of course, in reality you want it much bigger, so
 * the constraint on pagesize mod 256 is not an important restriction.
 * On the high end, we can only support pages up to 32KB because lp_off/lp_len
 * are 15 bits.
 */
 
typedef struct PageHeaderData
{
    /* XXX LSN is member of *any* block, not only page-organized ones */
    XLogRecPtr  pd_lsn;         /* LSN: next byte after last byte of xlog
                                 * record for last change to this page */
    uint16      pd_tli;         /* least significant bits of the TimeLineID
                                 * containing the LSN */
    uint16      pd_flags;       /* flag bits, see below */
    LocationIndex pd_lower;     /* offset to start of free space */
    LocationIndex pd_upper;     /* offset to end of free space */
    LocationIndex pd_special;   /* offset to start of special space */
    uint16      pd_pagesize_version;
    TransactionId pd_prune_xid; /* oldest prunable XID, or zero if none */
    ItemIdData  pd_linp[1];     /* beginning of line pointer array */
} PageHeaderData;
```