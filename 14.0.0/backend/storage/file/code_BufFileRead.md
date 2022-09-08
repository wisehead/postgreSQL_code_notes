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
```