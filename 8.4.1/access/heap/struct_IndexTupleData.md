#1.struct IndexTupleData

```cpp
/*
 * Index tuple header structure
 *
 * All index tuples start with IndexTupleData.  If the HasNulls bit is set,
 * this is followed by an IndexAttributeBitMapData.  The index attribute
 * values follow, beginning at a MAXALIGN boundary.
 *
 * Note that the space allocated for the bitmap does not vary with the number
 * of attributes; that is because we don't have room to store the number of
 * attributes in the header.  Given the MAXALIGN constraint there's no space
 * savings to be had anyway, for usual values of INDEX_MAX_KEYS.
 */

typedef struct IndexTupleData
{
    ItemPointerData t_tid;      /* reference TID to heap tuple */

    /* ---------------
     * t_info is layed out in the following fashion:
     *
     * 15th (high) bit: has nulls
     * 14th bit: has var-width attributes
     * 13th bit: unused
     * 12-0 bit: size of tuple
     * ---------------
     */

    unsigned short t_info;      /* various info about tuple */

} IndexTupleData;               /* MORE DATA FOLLOWS AT END OF STRUCT */

typedef IndexTupleData *IndexTuple;

```