#1.BufFileLoadBuffer

```
BufFileLoadBuffer
--if (file->curOffset >= MAX_PHYSICAL_FILESIZE &&
		file->curFile + 1 < file->numFiles)
----file->curFile++;
----file->curOffset = 0L;
--thisfile = file->files[file->curFile];
--FileRead
```