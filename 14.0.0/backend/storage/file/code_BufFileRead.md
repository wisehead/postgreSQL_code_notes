#1.BufFileRead

```
BufFileRead
--BufFileFlush
--while (size > 0)
----if (file->pos >= file->nbytes)
------file->curOffset += file->pos;
------file->pos = 0;
------file->nbytes = 0;
------BufFileLoadBuffer(file);
----nthistime = file->nbytes - file->pos;
----if (nthistime > size)
			nthistime = size;
----memcpy(ptr, file->buffer.data + file->pos, nthistime);
----file->pos += nthistime;
		ptr = (void *) ((char *) ptr + nthistime);
		size -= nthistime;
		nread += nthistime;
```