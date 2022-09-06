#1.struct BuildAccumulator


```cpp
typedef struct
{
    GinState   *ginstate;
    EntryAccumulator *entries;
    uint32      maxdepth;
    EntryAccumulator **stack;
    uint32      stackpos;
    long        allocatedMemory;

    uint32      length;
    EntryAccumulator *entryallocator;
} BuildAccumulator;
```