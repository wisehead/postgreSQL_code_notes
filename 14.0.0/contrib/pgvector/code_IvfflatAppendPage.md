#1.IvfflatAppendPage

```
IvfflatAppendPage
--IvfflatNewBuffer
--GenericXLogRegisterBuffer
--IvfflatPageGetOpaque(*page)->nextblkno = BufferGetBlockNumber(newbuf);
--IvfflatInitPage
--MarkBufferDirty(*buf);
--MarkBufferDirty(newbuf);
--GenericXLogFinish(*state);
--UnlockReleaseBuffer(*buf);
--*state = GenericXLogStart(index);
--*page = GenericXLogRegisterBuffer(*state, newbuf, GENERIC_XLOG_FULL_IMAGE);
```