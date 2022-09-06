#1.struct GinBtreeData

```cpp
typedef struct GinBtreeData *GinBtree;

typedef struct GinBtreeData
{
    /* search methods */
    BlockNumber (*findChildPage) (GinBtree, GinBtreeStack *);
    bool        (*isMoveRight) (GinBtree, Page);
    bool        (*findItem) (GinBtree, GinBtreeStack *);

    /* insert methods */
    OffsetNumber (*findChildPtr) (GinBtree, Page, BlockNumber, OffsetNumber);
    BlockNumber (*getLeftMostPage) (GinBtree, Page);
    bool        (*isEnoughSpace) (GinBtree, Buffer, OffsetNumber);
    void        (*placeToPage) (GinBtree, Buffer, OffsetNumber, XLogRecData **);
    Page        (*splitPage) (GinBtree, Buffer, Buffer, OffsetNumber, XLogRecData **);
    void        (*fillRoot) (GinBtree, Buffer, Buffer, Buffer);

    bool        searchMode;

    Relation    index;
    GinState   *ginstate;
    bool        fullScan;
    bool        isBuild;

    BlockNumber rightblkno;

    /* Entry options */
    OffsetNumber entryAttnum;
    Datum       entryValue;
    IndexTuple  entry;
    bool        isDelete;

    /* Data (posting tree) option */
    ItemPointerData *items;
    uint32      nitem;
    uint32      curitem;

    PostingItem pitem;
} GinBtreeData;


```