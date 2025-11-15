#1.upate

BEGIN;
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
UPDATE parent SET b1=10 WHERE pid=2; //读取索引，执行UPDATE


首先，在SQL的分析阶段，即调用relation_open()函数但不调用LockRelationOid()函数，此时不对表加锁。调用栈类似SELECT语句。但请注意，更新语句这个示例，没有列出不加锁和在系统表对象上加锁的情况，因此如下过程看似比上一个示例步骤少且简单。

#2
首先，在SQL的分析阶段，即调用relation_open()函数但不调用LockRelationOid()函数，此时不对表加锁。调用栈类似SELECT语句。但请注意，更新语句这个示例，没有列出不加锁和在系统表对象上加锁的情况，因此如下过程看似比上一个示例步骤少且简单。

```cpp
PostgresMain()
  exec_simple_query()
    pg_analyze_and_rewrite()  //逻辑优化阶段的查询重写
      pg_rewrite_query()
        QueryRewrite()
          fireRIRrules()
            heap_open()
              relation_open()
                LockRelationOid(relationId, lockmode=1=AccessShareLock)
                //此时为表/视图即RELATION加锁
```

#3
第三，在生成执行计划的时候，要读取索引，此时因UPDATE语句中使用索引，在索引上施加了RowExclusiveLock锁：

```cpp
PostgresMain()
  exec_simple_query()
    pg_plan_queries()
      pg_plan_query()
        planner()
          standard_planner()
            subquery_planner() //生成执行计划
              grouping_planner()
                query_planner()
                  add_base_rels_to_query()
                    add_base_rels_to_query()
                      build_simple_rel()
                        get_relation_info()
                          index_open() //打开索引
                            relation_open()
                              LockRelationOid(relationId, lockmode=3=RowExclusiveLock)
                              //此时为表/视图即RELATION加锁
                              
```

#4.
第四，执行阶段，在关系山上施加了RowExclusiveLock锁：

```cpp
PostgresMain()
  exec_simple_query()
    PortalRun()
      PortalRunMulti()
        ProcessQuery()
          ExecutorStart() //执行器开始执行
            standard_ExecutorStart()
              InitPlan()
                heap_open()
                  relation_open()
                    LockRelationOid(relationId, lockmode=3=RowExclusiveLock)
                    //此时为表/视图即RELATION加锁
```