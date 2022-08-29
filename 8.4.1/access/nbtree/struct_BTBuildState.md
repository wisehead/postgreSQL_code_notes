#1.struct BTBuildState

```cpp
/* Working state for btbuild and its callback */
typedef struct
{
    bool        isUnique;
    bool        haveDead;
    Relation    heapRel;
    BTSpool    *spool;

    /*
     * spool2 is needed only when the index is an unique index. Dead tuples
     * are put into spool2 instead of spool in order to avoid uniqueness
     * check.
     */
    BTSpool    *spool2;
    double      indtuples;
} BTBuildState;

```