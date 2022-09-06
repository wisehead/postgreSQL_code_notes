#1.struct GinMetaPageData

```cpp
typedef struct GinMetaPageData
{
    /*
     * Pointers to head and tail of pending list, which consists of GIN_LIST
     * pages.  These store fast-inserted entries that haven't yet been moved
     * into the regular GIN structure.
     */
    BlockNumber head;
    BlockNumber tail;

    /*
     * Free space in bytes in the pending list's tail page.
     */
    uint32      tailFreeSize;

    /*
     * We store both number of pages and number of heap tuples that are in the
     * pending list.
     */
    BlockNumber nPendingPages;
    int64       nPendingHeapTuples;
} GinMetaPageData;

```