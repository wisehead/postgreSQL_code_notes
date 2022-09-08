#1.FileWrite

```
FileWrite
--FileAccess//lru touch hot
--vfdP = &VfdCache[file];
--pg_pwrite(VfdCache[file].fd, buffer, amount, offset);

```