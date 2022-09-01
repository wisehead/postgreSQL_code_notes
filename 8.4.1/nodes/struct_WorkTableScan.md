#1.struct WorkTableScan

```cpp
/* ----------------
 *      WorkTableScan node
 * ----------------
 */
typedef struct WorkTableScan
{
    Scan        scan;
    int         wtParam;        /* ID of Param representing work table */
} WorkTableScan;

```