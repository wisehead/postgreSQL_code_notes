#1.struct PREDICATELOCK

```cpp
typedef struct PREDICATELOCK  //谓词锁
{
    PREDICATELOCKTAG tag;        //谓词锁的唯一的标识，被当作hash key以唯一标识谓词锁
    SHM_QUEUE    targetLink;     /* list link in PREDICATELOCKTARGET's list of
                                       predicate locks */
    SHM_QUEUE    xactLink;       /* list link in SERIALIZABLEXACT's list of
                                    predicate locks */
    SerCommitSeqNo commitSeqNo;  /* only used for summarized predicate locks */
    //“summarized predicate”，汇总事务，是为节约内存谓词锁表而采取把已经提交了的多个事务
    的相关谓词汇集组合到一个“dummy”事务上
} PREDICATELOCK;
```