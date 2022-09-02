#1.struct CheckPoint

```cpp

/*
 * Body of CheckPoint XLOG records.  This is declared here because we keep
 * a copy of the latest one in pg_control for possible disaster recovery.
 */
typedef struct CheckPoint
{
    XLogRecPtr  redo;           /* next RecPtr available when we began to
                                 * create CheckPoint (i.e. REDO start point) */
    TimeLineID  ThisTimeLineID; /* current TLI */
    uint32      nextXidEpoch;   /* higher-order bits of nextXid */
    TransactionId nextXid;      /* next free XID */
    Oid         nextOid;        /* next free OID */
    MultiXactId nextMulti;      /* next free MultiXactId */
    MultiXactOffset nextMultiOffset;    /* next free MultiXact offset */
    pg_time_t   time;           /* time stamp of checkpoint */
} CheckPoint;
```