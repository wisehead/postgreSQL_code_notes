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
----LWLockRelease(newPartitionLock);
----return buf;
--LWLockRelease(newPartitionLock);
--for (;;)
----ReservePrivateRefCountEntry
----buf = StrategyGetBuffer(strategy, &buf_state);
----oldFlags = buf_state & BUF_FLAG_MASK;
----PinBuffer_Locked(buf);
```