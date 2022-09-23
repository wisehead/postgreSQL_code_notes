#1.struct FreePageManager

```cpp
/* Everything we need in order to manage free pages (see freepage.c) */
struct FreePageManager
{
	RelptrFreePageManager self;
	RelptrFreePageBtree btree_root;
	RelptrFreePageSpanLeader btree_recycle;
	unsigned	btree_depth;
	unsigned	btree_recycle_count;
	Size		singleton_first_page;
	Size		singleton_npages;
	Size		contiguous_pages;
	bool		contiguous_pages_dirty;
	RelptrFreePageSpanLeader freelist[FPM_NUM_FREELISTS];
#ifdef FPM_EXTRA_ASSERTS
	/* For debugging only, pages put minus pages gotten. */
	Size		free_pages;
#endif
};
```