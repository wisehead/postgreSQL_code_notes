#1.ginInsertCleanup

```cpp
6.为索引页加锁
ginInsertCleanup(GinState *ginstate, ...) //为指定的GIN索引页加锁，本质上类似于为指定的Relation加锁
{
    Relation index = ginstate->index; //一个index页，也是一个被“Relation”定义的对象
    if (inVacuum)
    {
        //We are called from [auto]vacuum/analyze or gin_clean_pending_list() and
          we would like to wait concurrent cleanup to finish.
        LockPage(index, GIN_METAPAGE_BLKNO, ExclusiveLock); //为指定的索引页加排它锁
...
    }
    else
    {
        //We are called from regular insert and if we see concurrent cleanup just
          exit in hope that concurrent process will clean up pending list.
        if (!ConditionalLockPage(index, GIN_METAPAGE_BLKNO, ExclusiveLock))
            return;
...
    }
...
}

```