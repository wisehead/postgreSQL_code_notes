#1.struct BTWriteState

```cpp
/*
 * Overall status record for index writing phase.
 */
typedef struct BTWriteState
{
    Relation    index;
    bool        btws_use_wal;   /* dump pages to WAL? */
    BlockNumber btws_pages_alloced;     /* # pages allocated */
    BlockNumber btws_pages_written;     /* # pages written out */
    Page        btws_zeropage;  /* workspace for filling zeroes */
} BTWriteState;
```