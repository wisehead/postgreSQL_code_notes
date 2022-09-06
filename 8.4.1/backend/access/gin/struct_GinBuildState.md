#1.struct GinBuildState

```cpp
typedef struct
{
    GinState    ginstate;
    double      indtuples;
    MemoryContext tmpCtx;
    MemoryContext funcCtx;
    BuildAccumulator accum;
} GinBuildState;

```