#1.struct BitmapHeapScan


```cpp
/* ----------------
 *      bitmap sequential scan node
 *
 * This needs a copy of the qual conditions being used by the input index
 * scans because there are various cases where we need to recheck the quals;
 * for example, when the bitmap is lossy about the specific rows on a page
 * that meet the index condition.
 * ----------------
 */
typedef struct BitmapHeapScan
{
    Scan        scan;
    List       *bitmapqualorig; /* index quals, in standard expr form */
} BitmapHeapScan;

```