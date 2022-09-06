#1.struct XLogRecPtr

```cpp
/*
 * Pointer to a location in the XLOG.  These pointers are 64 bits wide,
 * because we don't want them ever to overflow.
 *
 * NOTE: xrecoff == 0 is used to indicate an invalid pointer.  This is OK
 * because we use page headers in the XLOG, so no XLOG record can start
 * right at the beginning of a file.
 *
 * NOTE: the "log file number" is somewhat misnamed, since the actual files
 * making up the XLOG are much smaller than 4Gb.  Each actual file is an
 * XLogSegSize-byte "segment" of a logical log file having the indicated
 * xlogid.  The log file number and segment number together identify a
 * physical XLOG file.  Segment number and offset within the physical file
 * are computed from xrecoff div and mod XLogSegSize.
 */
typedef struct XLogRecPtr
{
    uint32      xlogid;         /* log file #, 0 based */
    uint32      xrecoff;        /* byte offset of location in log file */
} XLogRecPtr;
```