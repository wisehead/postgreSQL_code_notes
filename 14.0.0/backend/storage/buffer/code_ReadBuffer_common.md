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
```