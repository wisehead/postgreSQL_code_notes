	#1.AllocSetContextCreateInternal

```
AllocSetContextCreateInternal
--if (freeListIndex >= 0)
----AllocSetFreeList *freelist = &context_freelists[freeListIndex];
----if (freelist->first_free != NULL)
------set = freelist->first_free;
------freelist->first_free = (AllocSet) set->header.nextchild;
------freelist->num_free--;
------MemoryContextCreate
------return (MemoryContext) set;
--set = (AllocSet) malloc(firstBlockSize);
--block = (AllocBlock) (((char *) set) + MAXALIGN(sizeof(AllocSetContext)));
--MemoryContextCreate
```