#1.struct SnapshotData

```cpp
typedef struct SnapshotData
{
    SnapshotSatisfiesFunc satisfies;    /* tuple test function */

    /*
     * The remaining fields are used only for MVCC snapshots, and are normally
     * just zeroes in special snapshots.  (But xmin and xmax are used
     * specially by HeapTupleSatisfiesDirty.)
     *
     * An MVCC snapshot can never see the effects of XIDs >= xmax. It can see
     * the effects of all older XIDs except those listed in the snapshot. xmin
     * is stored as an optimization to avoid needing to search the XID arrays
     * for most tuples.
     */
    TransactionId xmin;         /* all XID < xmin are visible to me */
    TransactionId xmax;         /* all XID >= xmax are invisible to me */
    uint32      xcnt;           /* # of xact ids in xip[] */
    TransactionId *xip;         /* array of xact IDs in progress */
    /* note: all ids in xip[] satisfy xmin <= xip[i] < xmax */
    int32       subxcnt;        /* # of xact ids in subxip[], -1 if overflow */
    TransactionId *subxip;      /* array of subxact IDs in progress */

    /*
     * note: all ids in subxip[] are >= xmin, but we don't bother filtering
     * out any that are >= xmax
     */
    CommandId   curcid;         /* in my xact, CID < curcid are visible */
    uint32      active_count;   /* refcount on ActiveSnapshot stack */
    uint32      regd_count;     /* refcount on RegisteredSnapshotList */
    bool        copied;         /* false if it's a static snapshot */
} SnapshotData;
```