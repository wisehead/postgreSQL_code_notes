#1.btvacuumscan

```cpp
btvacuumscan(...)  //Btree树（B+树索引）上进行VACUUM类操作
{...
    for (;;)
    {
        /* Get the current relation length */
        if (needLock)
            LockRelationForExtension(rel, ExclusiveLock);
        num_pages = RelationGetNumberOfBlocks(rel);
        if (needLock)
            UnlockRelationForExtension(rel, ExclusiveLock);
...
   }
...
}

```