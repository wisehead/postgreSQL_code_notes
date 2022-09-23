#1.ReadBuffer_common

```
ReadBuffer_common
--isLocalBuf = SmgrIsTemp(smgr);
--isExtend = (blockNum == P_NEW);
--if (isExtend)
----smgrnblocks
------smgrnblocks_cached
------result = smgrsw[reln->smgr_which].smgr_nblocks(reln, forknum);
--------mdnblocks
--if (isLocalBuf)
----bufHdr = LocalBufferAlloc(smgr, forkNum, blockNum, &found);
--else
----BufferAlloc
--if (found)
----if (!isExtend)
------if (!isLocalBuf)
--------LWLockAcquire(BufferDescriptorGetContentLock(bufHdr),
								  LW_EXCLUSIVE);
------return BufferDescriptorGetBuffer(bufHdr);
----bufBlock = isLocalBuf ? LocalBufHdrGetBlock(bufHdr) : BufHdrGetBlock(bufHdr);
----if (isLocalBuf)
------buf_state &= ~BM_VALID;
----else
------buf_state &= ~BM_VALID;
--if (isExtend)
----smgrextend
--else
----if (mode == RBM_ZERO_AND_LOCK || mode == RBM_ZERO_AND_CLEANUP_LOCK)
------MemSet((char *) bufBlock, 0, BLCKSZ);
----else
------smgrread(smgr, forkNum, blockNum, (char *) bufBlock);
------PageIsVerifiedExtended
--if (isLocalBuf)
----buf_state |= BM_VALID;
--else
----TerminateBufferIO(bufHdr, false, BM_VALID);
```