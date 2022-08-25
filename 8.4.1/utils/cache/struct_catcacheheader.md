#1.struct catcacheheader

```cpp
typedef struct catcacheheader
{
    CatCache   *ch_caches;      /* head of list of CatCache structs */
    int         ch_ntup;        /* # of tuples in all caches */
} CatCacheHeader;
```