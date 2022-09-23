#1.BufferAlloc

```
BufferAlloc
--newHash = BufTableHashCode(&newTag);
--newPartitionLock = BufMappingPartitionLock(newHash);
--LWLockAcquire(newPartitionLock, LW_SHARED);
--buf_id = BufTableLookup(&newTag, newHash);
----hash_search_with_hash_value
--if (buf_id >= 0)
----buf = GetBufferDescriptor(buf_id);
----PinBuffer
```