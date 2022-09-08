#1.AllocateVfd

```
AllocateVfd
--if (VfdCache[0].nextFree == 0)
----newCacheSize = SizeVfdCache * 2;
----newVfdCache = (Vfd *) realloc(VfdCache, sizeof(Vfd) * newCacheSize);
----VfdCache = newVfdCache;
----for (i = SizeVfdCache; i < newCacheSize; i++)
		{
			MemSet((char *) &(VfdCache[i]), 0, sizeof(Vfd));
			VfdCache[i].nextFree = i + 1;
			VfdCache[i].fd = VFD_CLOSED;
		}
----VfdCache[newCacheSize - 1].nextFree = 0;
----VfdCache[0].nextFree = SizeVfdCache;
----SizeVfdCache = newCacheSize;
--file = VfdCache[0].nextFree;
--VfdCache[0].nextFree = VfdCache[file].nextFree;
```