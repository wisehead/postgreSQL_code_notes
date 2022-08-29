#1.struct BTMetaPageData

```cpp
/*
 * The Meta page is always the first page in the btree index.
 * Its primary purpose is to point to the location of the btree root page.
 * We also point to the "fast" root, which is the current effective root;
 * see README for discussion.
 */

typedef struct BTMetaPageData
{
    uint32      btm_magic;      /* should contain BTREE_MAGIC */
    uint32      btm_version;    /* should contain BTREE_VERSION */
    BlockNumber btm_root;       /* current root location */
    uint32      btm_level;      /* tree level of the root page */
    BlockNumber btm_fastroot;   /* current "fast" root location */
    uint32      btm_fastlevel;  /* tree level of the "fast" root page */
} BTMetaPageData;


```