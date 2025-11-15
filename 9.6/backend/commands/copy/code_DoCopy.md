#1.DoCopy

```cpp
DoCopy(const CopyStmt *stmt, const char *queryString, uint64 *processed)
{...
    if (stmt->relation)
    {...
        /* Open and lock the relation, using the appropriate lock type. */
        rel = heap_openrv(stmt->relation,
                          (is_from ? RowExclusiveLock : AccessShareLock));
                          //如果是从表中COPY数据到数据库外面，则使用共享锁
     ...}
...
    if (rel != NULL)
        heap_close(rel, (is_from ? NoLock : AccessShareLock));
...}

```