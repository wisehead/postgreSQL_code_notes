#1.struct VacPageData

```cpp
/*
 * VacPage structures keep track of each page on which we find useful
 * amounts of free space.
 */
typedef struct VacPageData
{
    BlockNumber blkno;          /* BlockNumber of this Page */
    Size        free;           /* FreeSpace on this Page */
    uint16      offsets_used;   /* Number of OffNums used by vacuum */
    uint16      offsets_free;   /* Number of OffNums free or to be free */
    OffsetNumber offsets[1];    /* Array of free OffNums */
} VacPageData;

typedef VacPageData *VacPage;


```