#1.struct SlruCtlData

```cpp
/*
 * SlruCtlData is an unshared structure that points to the active information
 * in shared memory.
 */
typedef struct SlruCtlData
{
    SlruShared  shared;

    /*
     * This flag tells whether to fsync writes (true for pg_clog and multixact
     * stuff, false for pg_subtrans).
     */
    bool        do_fsync;

    /*
     * Decide which of two page numbers is "older" for truncation purposes. We
     * need to use comparison of TransactionIds here in order to do the right
     * thing with wraparound XID arithmetic.
     */
    bool        (*PagePrecedes) (int, int);

    /*
     * Dir is set during SimpleLruInit and does not change thereafter. Since
     * it's always the same, it doesn't need to be in shared memory.
     */
    char        Dir[64];
} SlruCtlData;
```