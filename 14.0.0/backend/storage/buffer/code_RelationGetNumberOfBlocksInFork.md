#1.RelationGetNumberOfBlocksInFork

```cpp
/*
 * RelationGetNumberOfBlocksInFork
 *      Determines the current number of pages in the specified relation fork.
 *
 * Note that the accuracy of the result will depend on the details of the
 * relation's storage. For builtin AMs it'll be accurate, but for external AMs
 * it might not be.
 */
BlockNumber
RelationGetNumberOfBlocksInFork(Relation relation, ForkNumber forkNum)
```