#1.struct GISTInsertStack

```cpp
/*
 * GISTInsertStack used for locking buffers and transfer arguments during
 * insertion
 */

typedef struct GISTInsertStack
{
    /* current page */
    BlockNumber blkno;
    Buffer      buffer;
    Page        page;

    /*
     * log sequence number from page->lsn to recognize page update  and
     * compare it with page's nsn to recognize page split
     */
    GistNSN     lsn;

    /* child's offset */
    OffsetNumber childoffnum;

    /* pointer to parent and child */
    struct GISTInsertStack *parent;
    struct GISTInsertStack *child;

    /* for gistFindPath */
    struct GISTInsertStack *next;
} GISTInsertStack;
```