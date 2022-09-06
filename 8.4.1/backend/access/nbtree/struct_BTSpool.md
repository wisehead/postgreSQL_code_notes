#1.struct BTSpool

```cpp
/*
 * Status record for spooling/sorting phase.  (Note we may have two of
 * these due to the special requirements for uniqueness-checking with
 * dead tuples.)
 */
struct BTSpool
{
    Tuplesortstate *sortstate;  /* state data for tuplesort.c */
    Relation    index;
    bool        isunique;
};

```