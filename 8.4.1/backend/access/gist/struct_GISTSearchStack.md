#1.struct GISTSearchStack

```cpp
/*
 * XXX old comment!!!
 * When we descend a tree, we keep a stack of parent pointers. This
 * allows us to follow a chain of internal node points until we reach
 * a leaf node, and then back up the stack to re-examine the internal
 * nodes.
 *
 * 'parent' is the previous stack entry -- i.e. the node we arrived
 * from. 'block' is the node's block number. 'offset' is the offset in
 * the node's page that we stopped at (i.e. we followed the child
 * pointer located at the specified offset).
 */
typedef struct GISTSearchStack
{
    struct GISTSearchStack *next;
    BlockNumber block;
    /* to identify page changed */
    GistNSN     lsn;
    /* to recognize split occured */
    GistNSN     parentlsn;
} GISTSearchStack;

```