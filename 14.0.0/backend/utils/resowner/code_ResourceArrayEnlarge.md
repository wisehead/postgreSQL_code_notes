#1.ResourceArrayEnlarge

```
ResourceArrayEnlarge
--if (resarr->nitems < resarr->maxitems)
----return
--newcap = (oldcap > 0) ? oldcap * 2 : RESARRAY_INIT_SIZE;
--newitemsarr = (Datum *) MemoryContextAlloc(TopMemoryContext,
											   newcap * sizeof(Datum));
--for (i = 0; i < newcap; i++)
		newitemsarr[i] = resarr->invalidval;
--resarr->itemsarr = newitemsarr;
--resarr->capacity = newcap;
--resarr->maxitems = RESARRAY_MAX_ITEMS(newcap);
--resarr->nitems = 0;
--if (olditemsarr != NULL)
----for (i = 0; i < oldcap; i++)
------if (olditemsarr[i] != resarr->invalidval)
--------ResourceArrayAdd(resarr, olditemsarr[i]);
------pfree(olditemsarr);
```

#2.ResourceArrayAdd

```cpp
ResourceArrayAdd
--if (RESARRAY_IS_ARRAY(resarr))//数组长度小于64，线性数组
----idx = resarr->nitems;
--else
----//循环数组，有GC机制

```