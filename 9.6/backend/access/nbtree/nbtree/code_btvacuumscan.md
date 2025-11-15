#1.btvacuumscan
2.对于诸如VACUUM类似的操作在指定对象上加排它锁

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