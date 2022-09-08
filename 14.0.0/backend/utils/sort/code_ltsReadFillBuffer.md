#1.ltsReadFillBuffer

```
ltsReadFillBuffer
--do
----thisbuf = lt->buffer + lt->nbytes;
----datablocknum = lt->nextBlockNumber;
----datablocknum += lt->offsetBlockNumber;
----ltsReadBlock(lts, datablocknum, (void *) thisbuf);
------BufFileSeekBlock
--------BufFileSeek
----------if (newFile == file->curFile && newOffset >= file->curOffset && newOffset <= file->curOffset + file->nbytes)
------------
------BufFileRead


--while (lt->buffer_size - lt->nbytes > BLCKSZ);
```