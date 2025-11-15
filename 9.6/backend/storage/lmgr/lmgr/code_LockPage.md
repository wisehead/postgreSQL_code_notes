#1.LockPage
页面级别的锁，简称页锁，用于控制对共享缓冲池中索引的页面的读/写并发访问、以及在GIN索引上执行VACUUM操作，可用的场景较少，有共享、排他两种锁的模式。前面谈到的锁的类型时，所提出的LOCKTAG_PAGE正是页锁的标识。页锁被数据库的事务管理系统自动施加，在事务提交或回滚阶段自动释放。页锁的加锁和释放锁类似表锁的封锁过程，也是调用LockAcquire()函数和LockRelease()函数完成。


```cpp
void
LockPage(Relation relation, BlockNumber blkno, LOCKMODE lockmode)  //加页锁
{...
    SET_LOCKTAG_PAGE(tag, relation->rd_lockInfo.lockRelId.dbId, relation->
    rd_lockInfo.lockRelId.relId, blkno);
    (void) LockAcquire(&tag, lockmode, false, false);
}
UnlockPage(Relation relation, BlockNumber blkno, LOCKMODE lockmode)  //释放页锁
{...
    SET_LOCKTAG_PAGE(tag, relation->rd_lockInfo.lockRelId.dbId, relation->
    rd_lockInfo.lockRelId.relId,  blkno);
    LockRelease(&tag, lockmode, false);
}
```