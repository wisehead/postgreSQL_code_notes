#1. BufFileSeek

```
BufFileSeek
--if (newFile == file->curFile && newOffset >= file->curOffset && newOffset <= file->curOffset + file->nbytes)
----file->pos = (int) (newOffset - file->curOffset);
----return 0
--BufFileFlush
----BufFileDumpBuffer

```