#1.ltsReleaseBlock

```
ltsReleaseBlock
--if (lts->nFreeBlocks >= lts->freeBlocksLen)
----lts->freeBlocksLen *= 2;
----lts->freeBlocks = (long *) repalloc(lts->freeBlocks,
											lts->freeBlocksLen * sizeof(long));
--heap = lts->freeBlocks;
--pos = lts->nFreeBlocks;

--heap[pos] = blocknum;
--lts->nFreeBlocks++;
--while (pos != 0)//min heap
	{
		unsigned long parent = parent_offset(pos);

		if (heap[parent] < heap[pos])
			break;

		swap_nodes(heap, parent, pos);
		pos = parent;
--}
```