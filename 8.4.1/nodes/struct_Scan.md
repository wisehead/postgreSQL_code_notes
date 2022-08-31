#1.struct Scan

```cpp
/*
 * ==========
 * Scan nodes
 * ==========
 */
typedef struct Scan
{
    Plan        plan;
    Index       scanrelid;      /* relid is index into the range table */
} Scan;

/* ----------------
 *      sequential scan node
 * ----------------
 */
typedef Scan SeqScan;
```