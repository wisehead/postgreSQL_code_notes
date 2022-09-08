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
------BufFileRead


--while (lt->buffer_size - lt->nbytes > BLCKSZ);
```

