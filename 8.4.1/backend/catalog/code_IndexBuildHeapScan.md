#1.IndexBuildHeapScan


```cpp

/*
 * IndexBuildHeapScan - scan the heap relation to find tuples to be indexed
 *
 * This is called back from an access-method-specific index build procedure
 * after the AM has done whatever setup it needs.  The parent heap relation
 * is scanned to find tuples that should be entered into the index.  Each
 * such tuple is passed to the AM's callback routine, which does the right
 * things to add it to the new index.  After we return, the AM's index
 * build procedure does whatever cleanup is needed; in particular, it should
 * close the heap and index relations.
 *
 * The total count of heap tuples is returned.  This is for updating pg_class
 * statistics.  (It's annoying not to be able to do that here, but we can't
 * do it until after the relation is closed.)  Note that the index AM itself
 * must keep track of the number of index tuples; we don't do so here because
 * the AM might reject some of the tuples for its own reasons, such as being
 * unable to store NULLs.
 *
 * A side effect is to set indexInfo->ii_BrokenHotChain to true if we detect
 * any potentially broken HOT chains.  Currently, we set this if there are
 * any RECENTLY_DEAD or DELETE_IN_PROGRESS entries in a HOT chain, without
 * trying very hard to detect whether they're really incompatible with the
 * chain tip.
 */
double
IndexBuildHeapScan(Relation heapRelation,
                   Relation indexRelation,
                   IndexInfo *indexInfo,
                   bool allow_sync,
                   IndexBuildCallback callback,
                   void *callback_state)
```