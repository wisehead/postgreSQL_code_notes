#1.struct GinBtreeStack

```cpp
/* ginbtree.c */

typedef struct GinBtreeStack
{
    BlockNumber blkno;
    Buffer      buffer;
    OffsetNumber off;
    /* predictNumber contains prediction number of pages on current level */
    uint32      predictNumber;
    struct GinBtreeStack *parent;
} GinBtreeStack;

```