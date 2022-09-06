#1.SampleRows

```cpp
/*
 * Sample rows with same logic as ANALYZE
 */
SampleRows
--RelationGetNumberOfBlocks
----RelationGetNumberOfBlocksInFork
--UpdateProgress
----pgstat_progress_update_param
--BlockSampler_Init

```