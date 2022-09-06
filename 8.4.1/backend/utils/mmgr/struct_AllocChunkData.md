#1.struct AllocChunkData

```cpp

/*
 * AllocChunk
 *      The prefix of each piece of memory in an AllocBlock
 *
 * NB: this MUST match StandardChunkHeader as defined by utils/memutils.h.
 */
typedef struct AllocChunkData
{
    /* aset is the owning aset if allocated, or the freelist link if free */
    void       *aset;
    /* size is always the size of the usable space in the chunk */
    Size        size;
#ifdef MEMORY_CONTEXT_CHECKING
    /* when debugging memory usage, also store actual requested size */
    /* this is zero in a free chunk */
    Size        requested_size;
#endif
} AllocChunkData;
```