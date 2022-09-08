#1.extendBufFile

```
extendBufFile
--if (file->fileset == NULL)
----pfile = OpenTemporaryFile(file->isInterXact);
--else
----pfile = MakeNewSharedSegment(file, file->numFiles);
--file->files = (File *) repalloc(file->files,
									(file->numFiles + 1) * sizeof(File));
--file->files[file->numFiles] = pfile;
--file->numFiles++;
```