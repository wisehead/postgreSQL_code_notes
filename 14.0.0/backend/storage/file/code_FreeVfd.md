#1.FreeVfd

```cpp
FreeVfd
--Vfd		   *vfdP = &VfdCache[file];
--free(vfdP->fileName);
--vfdP->fdstate = 0x0;

--vfdP->nextFree = VfdCache[0].nextFree;
--VfdCache[0].nextFree = file;
```