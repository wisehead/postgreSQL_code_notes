#1.GetPrivateRefCountEntry

```
GetPrivateRefCountEntry
--for (i = 0; i < REFCOUNT_ARRAY_ENTRIES; i++)
	{
		res = &PrivateRefCountArray[i];

		if (res->buffer == buffer)
			return res;
--}
--hash_search(PrivateRefCountHash,
					  (void *) &buffer,
					  HASH_FIND,
					  NULL);
```