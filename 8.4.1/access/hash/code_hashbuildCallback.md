#1.hashbuildCallback

```cpp
/*
 * Per-tuple callback from IndexBuildHeapScan
 */
static void
hashbuildCallback(Relation index,
                  HeapTuple htup,
                  Datum *values,
                  bool *isnull,
                  bool tupleIsAlive,
                  void *state)

```