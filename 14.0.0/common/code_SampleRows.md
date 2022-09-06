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
--reservoir_init_selection_state
----sampler_random_init_state
--while (BlockSampler_HasMore(&buildstate->bs))
----BlockNumber targblock = BlockSampler_Next(&buildstate->bs);
----table_index_build_range_scan
------table_rel->rd_tableam->index_build_range_scan


```