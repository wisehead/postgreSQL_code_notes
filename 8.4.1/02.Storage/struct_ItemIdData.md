#1.struct ItemIdData

```cpp

/*
 * An item pointer (also called line pointer) on a buffer page
 *
 * In some cases an item pointer is "in use" but does not have any associated
 * storage on the page.  By convention, lp_len == 0 in every item pointer
 * that does not have storage, independently of its lp_flags state.
 */
typedef struct ItemIdData
{
    unsigned    lp_off:15,      /* offset to tuple (from start of page) */
                lp_flags:2,     /* state of item pointer, see below */
                lp_len:15;      /* byte length of tuple */
} ItemIdData;

typedef ItemIdData *ItemId;
```