#1.ShmemInitStruct

```
ShmemInitStruct
--LWLockAcquire(ShmemIndexLock, LW_EXCLUSIVE);
--if (!ShmemIndex)//初始状态
----PGShmemHeader *shmemseghdr = ShmemSegHdr;
----if (IsUnderPostmaster)
------structPtr = shmemseghdr->index;
------*foundPtr = true;
----else
------structPtr = ShmemAlloc(size);
------shmemseghdr->index = structPtr;
------*foundPtr = false;
----LWLockRelease(ShmemIndexLock);
----return structPtr;
--result = (ShmemIndexEnt *)hash_search(ShmemIndex, name, HASH_ENTER_NULL, foundPtr);
--if (*foundPtr)
----structPtr = result->location;
--else
----structPtr = ShmemAllocRaw(size, &allocated_size);
----result->size = size;
----result->allocated_size = allocated_size;
----result->location = structPtr;
--LWLockRelease(ShmemIndexLock);
--return structPtr;
```

#2. ShmemAlloc
```
ShmemAlloc
--ShmemAllocRaw
```


#3. ShmemAllocRaw

```
ShmemAllocRaw
--size = CACHELINEALIGN(size);
--SpinLockAcquire(ShmemLock);
--newStart = ShmemSegHdr->freeoffset;
--newFree = newStart + size;
--if (newFree <= ShmemSegHdr->totalsize)
----newSpace = (void *) ((char *) ShmemBase + newStart);
----ShmemSegHdr->freeoffset = newFree;
--else
----newSpace = NULL;
--SpinLockRelease(ShmemLock);
```






