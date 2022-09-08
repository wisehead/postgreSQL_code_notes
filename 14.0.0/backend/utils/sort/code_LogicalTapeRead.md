#1.LogicalTapeRead

```
LogicalTapeRead                                                  
--ltsInitReadBuffer(lts, lt);                
----lt->buffer = palloc(lt->buffer_size);    
----ltsReadFillBuffer                        
--while (size > 0)
----if (lt->pos >= lt->nbytes)
------ltsReadFillBuffer
----nthistime = lt->nbytes - lt->pos;
----if (nthistime > size)
			nthistime = size;
----memcpy(ptr, lt->buffer + lt->pos, nthistime);
----lt->pos += nthistime;
		ptr = (void *) ((char *) ptr + nthistime);
		size -= nthistime;
		nread += nthistime;
```