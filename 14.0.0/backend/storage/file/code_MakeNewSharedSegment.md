#1.MakeNewSharedSegment

```
MakeNewSharedSegment
--SharedSegmentName(name, buffile->name, segment + 1);
----snprintf(name, MAXPGPATH, "%s.%d", buffile_name, segment);
--SharedFileSetDelete(buffile->fileset, name, true);
--SharedSegmentName(name, buffile->name, segment);
--SharedFileSetCreate
----SharedFilePath
------SharedFileSetPath(dirpath, fileset, ChooseTablespace(fileset, name));
------snprintf(path, MAXPGPATH, "%s/%s", dirpath, name);
----PathNameCreateTemporaryFile
------PathNameOpenFile
```