#1.ScanQueryForLocks

```cpp
ScanQueryForLocks(Query *parsetree, bool acquire)
{...
    rt_index = 0;
    foreach(lc, parsetree->rtable) //遍历FROM子句中的每一个范围表
    {...
        switch (rte->rtekind)
        {
            case RTE_RELATION: //如果是用户表
                /* Acquire or release the appropriate type of lock */
                if (rt_index == parsetree->resultRelation)
                    lockmode = RowExclusiveLock;
                else if (get_parse_rowmark(parsetree, rt_index) != NULL)
                    lockmode = RowShareLock;  //SELECT...FOR UPDATE
                else
                    lockmode = AccessShareLock; //SELECT...FOR SHARE
                if (acquire)
                    LockRelationOid(rte->relid, lockmode);
                else
                    UnlockRelationOid(rte->relid, lockmode);
                break;
           case RTE_SUBQUERY: //如果是子查询，递归遍历
                /* Recurse into subquery-in-FROM */
                ScanQueryForLocks(rte->subquery, acquire);
                break;
            default:
                /* ignore other types of RTEs */
                break;
            }
        }
...}

```