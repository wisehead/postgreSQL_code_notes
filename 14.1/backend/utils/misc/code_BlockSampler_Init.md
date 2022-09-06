#1.BlockSampler_Init

```cpp
/*
 * BlockSampler_Init -- prepare for random sampling of blocknumbers
 *
 * BlockSampler provides algorithm for block level sampling of a relation
 * as discussed on pgsql-hackers 2004-04-02 (subject "Large DB")
 * It selects a random sample of samplesize blocks out of
 * the nblocks blocks in the table. If the table has less than
 * samplesize blocks, all blocks are selected.
 *
 * Since we know the total number of blocks in advance, we can use the
 * straightforward Algorithm S from Knuth 3.4.2, rather than Vitter's
 * algorithm.
 *
 * Returns the number of blocks that BlockSampler_Next will return.
 */
BlockSampler_Init
--sampler_random_init_state
--Min(bs->n, bs->N)
```