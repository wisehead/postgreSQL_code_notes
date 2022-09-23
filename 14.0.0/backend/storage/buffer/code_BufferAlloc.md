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
----if (oldFlags & BM_DIRTY)
------if (LWLockConditionalAcquire(BufferDescriptorGetContentLock(buf),
										 LW_SHARED))
--------FlushBuffer(buf, NULL);
--------LWLockRelease(BufferDescriptorGetContentLock(buf));
----else
------UnpinBuffer(buf, true);
------continue;
----if (oldFlags & BM_TAG_VALID)
------oldTag = buf->tag;
------oldHash = BufTableHashCode(&oldTag);
------oldPartitionLock = BufMappingPartitionLock(oldHash);
------LWLockAcquire(oldPartitionLock, LW_EXCLUSIVE);
------LWLockAcquire(newPartitionLock, LW_EXCLUSIVE);
----else
------LWLockAcquire(newPartitionLock, LW_EXCLUSIVE);
----buf_id = BufTableInsert(&newTag, newHash, buf->buf_id);
----if (buf_id >= 0)
------UnpinBuffer(buf, true);
------buf = GetBufferDescriptor(buf_id);
------valid = PinBuffer(buf, strategy);
------LWLockRelease(newPartitionLock);
------return buf;
----buf_state = LockBufHdr(buf);
----oldFlags = buf_state & BUF_FLAG_MASK;
----if (BUF_STATE_GET_REFCOUNT(buf_state) == 1 && !(oldFlags & BM_DIRTY))
------break;
----UnlockBufHdr(buf, buf_state);
----BufTableDelete(&newTag, newHash);
----LWLockRelease(newPartitionLock);
----UnpinBuffer(buf, true);
--//end for
--buf->tag = newTag;
--UnlockBufHdr(buf, buf_state);
--LWLockRelease(newPartitionLock);
--LWLockRelease(newPartitionLock);

```


















