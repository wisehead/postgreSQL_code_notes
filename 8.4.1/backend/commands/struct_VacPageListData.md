#1.struct VacPageListData

```cpp
typedef struct VacPageListData
{
    BlockNumber empty_end_pages;    /* Number of "empty" end-pages */
    int         num_pages;      /* Number of pages in pagedesc */
    int         num_allocated_pages;    /* Number of allocated pages in
                                         * pagedesc */
    VacPage    *pagedesc;       /* Descriptions of pages */
} VacPageListData;
```