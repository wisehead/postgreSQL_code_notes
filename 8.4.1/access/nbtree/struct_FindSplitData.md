#1.struct FindSplitData

```cpp
typedef struct
{
    /* context data for _bt_checksplitloc */
    Size        newitemsz;      /* size of new item to be inserted */
    int         fillfactor;     /* needed when splitting rightmost page */
    bool        is_leaf;        /* T if splitting a leaf page */
    bool        is_rightmost;   /* T if splitting a rightmost page */
    OffsetNumber newitemoff;    /* where the new item is to be inserted */
    int         leftspace;      /* space available for items on left page */
    int         rightspace;     /* space available for items on right page */
    int         olddataitemstotal;      /* space taken by old items */

    bool        have_split;     /* found a valid split? */

    /* these fields valid only if have_split is true */
    bool        newitemonleft;  /* new item on left or right of best split */
    OffsetNumber firstright;    /* best split point */
    int         best_delta;     /* best size delta so far */
} FindSplitData;
```