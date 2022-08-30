#1.struct BTStackData

```cpp
/*
 *  BTStackData -- As we descend a tree, we push the (location, downlink)
 *  pairs from internal pages onto a private stack.  If we split a
 *  leaf, we use this stack to walk back up the tree and insert data
 *  into parent pages (and possibly to split them, too).  Lehman and
 *  Yao's update algorithm guarantees that under no circumstances can
 *  our private stack give us an irredeemably bad picture up the tree.
 *  Again, see the paper for details.
 */

typedef struct BTStackData
{
    BlockNumber bts_blkno;
    OffsetNumber bts_offset;
    IndexTupleData bts_btentry;
    struct BTStackData *bts_parent;
} BTStackData;

typedef BTStackData *BTStack;
```