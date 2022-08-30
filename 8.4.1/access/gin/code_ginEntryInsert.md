#1.ginEntryInsert

```cpp
/*
 * Inserts only one entry to the index, but it can add more than 1 ItemPointer.
 */
void
ginEntryInsert(Relation index, GinState *ginstate,
               OffsetNumber attnum, Datum value,
               ItemPointerData *items, uint32 nitem,
               bool isBuild)
```