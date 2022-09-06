#1.struct EntryAccumulator

```cpp

/* ginbulk.c */
typedef struct EntryAccumulator
{
    OffsetNumber attnum;
    Datum       value;
    uint32      length;
    uint32      number;
    ItemPointerData *list;
    bool        shouldSort;
    struct EntryAccumulator *left;
    struct EntryAccumulator *right;
} EntryAccumulator;
```