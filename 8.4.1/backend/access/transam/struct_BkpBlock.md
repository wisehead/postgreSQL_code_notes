#1.struct BkpBlock

```cpp
/*
 * Header info for a backup block appended to an XLOG record.
 *
 * As a trivial form of data compression, the XLOG code is aware that
 * PG data pages usually contain an unused "hole" in the middle, which
 * contains only zero bytes.  If hole_length > 0 then we have removed
 * such a "hole" from the stored data (and it's not counted in the
 * XLOG record's CRC, either).  Hence, the amount of block data actually
 * present following the BkpBlock struct is BLCKSZ - hole_length bytes.
 *
 * Note that we don't attempt to align either the BkpBlock struct or the
 * block's data.  So, the struct must be copied to aligned local storage
 * before use.
 */
typedef struct BkpBlock
{
    RelFileNode node;           /* relation containing block */
    ForkNumber  fork;           /* fork within the relation */
    BlockNumber block;          /* block number */
    uint16      hole_offset;    /* number of bytes before "hole" */
    uint16      hole_length;    /* number of bytes in "hole" */

    /* ACTUAL BLOCK DATA FOLLOWS AT END OF STRUCT */
} BkpBlock;
```