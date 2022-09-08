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
----if (!lt->frozen)
------ltsReleaseBlock(lts, datablocknum);
----lt->curBlockNumber = lt->nextBlockNumber;
----lt->nbytes += TapeBlockGetNBytes(thisbuf);
----if (TapeBlockIsLast(thisbuf))
		{
			lt->nextBlockNumber = -1L;
			/* EOF */
			break;
		}
----else
			lt->nextBlockNumber = TapeBlockGetTrailer(thisbuf)->next;


--while (lt->buffer_size - lt->nbytes > BLCKSZ);
```

