#1.struct BTPageOpaqueData

```cpp
/*
 *  BTPageOpaqueData -- At the end of every page, we store a pointer
 *  to both siblings in the tree.  This is used to do forward/backward
 *  index scans.  The next-page link is also critical for recovery when
 *  a search has navigated to the wrong page due to concurrent page splits
 *  or deletions; see src/backend/access/nbtree/README for more info.
 *
 *  In addition, we store the page's btree level (counting upwards from
 *  zero at a leaf page) as well as some flag bits indicating the page type
 *  and status.  If the page is deleted, we replace the level with the
 *  next-transaction-ID value indicating when it is safe to reclaim the page.
 *
 *  We also store a "vacuum cycle ID".  When a page is split while VACUUM is
 *  processing the index, a nonzero value associated with the VACUUM run is
 *  stored into both halves of the split page.  (If VACUUM is not running,
 *  both pages receive zero cycleids.)  This allows VACUUM to detect whether
 *  a page was split since it started, with a small probability of false match
 *  if the page was last split some exact multiple of MAX_BT_CYCLE_ID VACUUMs
 *  ago.  Also, during a split, the BTP_SPLIT_END flag is cleared in the left
 *  (original) page, and set in the right page, but only if the next page
 *  to its right has a different cycleid.
 *
 *  NOTE: the BTP_LEAF flag bit is redundant since level==0 could be tested
 *  instead.
 */

typedef struct BTPageOpaqueData
{
    BlockNumber btpo_prev;      /* left sibling, or P_NONE if leftmost */
    BlockNumber btpo_next;      /* right sibling, or P_NONE if rightmost */
    union
    {
        uint32      level;      /* tree level --- zero for leaf pages */
        TransactionId xact;     /* next transaction ID, if deleted */
    }           btpo;
    uint16      btpo_flags;     /* flag bits, see below */
    BTCycleId   btpo_cycleid;   /* vacuum cycle ID of latest split */
} BTPageOpaqueData;

typedef BTPageOpaqueData *BTPageOpaque;

```