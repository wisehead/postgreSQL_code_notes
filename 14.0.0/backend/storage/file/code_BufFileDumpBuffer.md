#1.BufFileDumpBuffer

```
BufFileDumpBuffer
--while (wpos < file->nbytes)
----if (file->curOffset >= MAX_PHYSICAL_FILESIZE)
------extendBufFile(file);
----bytestowrite = file->nbytes - wpos;
----availbytes = MAX_PHYSICAL_FILESIZE - file->curOffset;
----if ((off_t) bytestowrite > availbytes)
------bytestowrite = (int) availbytes;
----thisfile = file->files[file->curFile];
----FileWrite
----file->curOffset += bytestowrite;
----wpos += bytestowrite;

```