#1.PathNameOpenFilePerm

```
PathNameOpenFilePerm
--fnamecopy = strdup(fileName);
--file = AllocateVfd();
--vfdP = &VfdCache[file];
--ReleaseLruFiles
----ReleaseLruFile
------LruDelete
--BasicOpenFilePerm
----fd = open(fileName, fileFlags, fileMode);
--Insert(file);
----vfdP->lruMoreRecently = 0;
	vfdP->lruLessRecently = VfdCache[0].lruLessRecently;
	VfdCache[0].lruLessRecently = file;
	VfdCache[vfdP->lruLessRecently].lruMoreRecently = file;
```

#2. ReleaseLruFiles
```
ReleaseLruFiles
--ReleaseLruFile
----LruDelete
------vfdP = &VfdCache[file];
------close(vfdP->fd) 
------vfdP->fd = VFD_CLOSED;
------ --nfile;
------Delete(file);
--------vfdP = &VfdCache[file];
--------VfdCache[vfdP->lruLessRecently].lruMoreRecently = vfdP->lruMoreRecently;
--------VfdCache[vfdP->lruMoreRecently].lruLessRecently = vfdP->lruLessRecently;


```