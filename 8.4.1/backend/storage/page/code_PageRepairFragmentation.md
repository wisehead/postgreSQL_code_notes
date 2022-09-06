#1.PageRepairFragmentation

```cpp
/*
 * PageRepairFragmentation
 *
 * Frees fragmented space on a page.
 * It doesn't remove unused line pointers! Please don't change this.
 *
 * This routine is usable for heap pages only, but see PageIndexMultiDelete.
 *
 * As a side effect, the page's PD_HAS_FREE_LINES hint bit is updated.
 */
void
PageRepairFragmentation(Page page)
```