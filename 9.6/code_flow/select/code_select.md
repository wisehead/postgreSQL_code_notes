#1. relation_open

```cpp
#以“SELECT*FROM parent；​”为例，观察加锁过程与调用的函数栈。

#1.首先，在SQL的分析阶段，即调用relation_open()函数但不调用LockRelationOid()函数，此时不对表加锁

PostgresMain()
  exec_simple_query()  //执行一个简单的查询语句
    pg_analyze_and_rewrite()
      parse_analyze()  //分析阶段
        transformTopLevelStmt()
          transformStmt()
            transformSelectStmt()
              transformFromClause()  //分析FROM子句
                transformFromClauseItem()
                  transformTableEntry()  //分析FROM子句的表/视图等对象
                    parserOpenTable()
                      heap_openrv_extended()
                        relation_openrv_extended()
                          relation_open()  //分析阶段即打开表，后续要读入表的列对象等，
                          尚不涉及事务，所以锁值为0
```


#2.其次，在分析阶段，当要获取元组的元信息的时候，为表/视图加AccessShareLock锁。

```cpp

PostgresMain()
  exec_simple_query()  //执行一个简单的查询语句
    pg_analyze_and_rewrite()
      parse_analyze()  //分析阶段
        transformTopLevelStmt()
          transformStmt()
            transformSelectStmt()  //分析FROM子句
              transformTargetList()  //分析目标列子句
                ExpandColumnRefStar()  //把目标列中的“*”扩展为列对象
                  ExpandAllTables()
                    expandRelAttrs()
                      expandRTE()
                        expandRelation()  //获取元组的描述信息，为表加AccessShareLock锁
                          relation_open()
                            LockRelationOid(relationId,lockmode=1=AccessShareLock)
                             //此时为表/视图即RELATION加AccessShareLock锁

```


#3.第三，在优化阶段打开表但不加锁；此过程可能执行多次；

```cpp
PostgresMain()
  exec_simple_query()  //执行一个简单的查询语句
    pg_analyze_and_rewrite()
      pg_rewrite_query()  //查询重写，即逻辑优化阶段
        QueryRewrite()
          fireRIRrules() //对于每一个范围表应用RIR规则
            heap_open()
              relation_open() //此时为表/视图即RELATION不加锁

```

#4.第四，生成执行计划时，无锁方式打开RELATION；

```cpp
PostgresMain()
  exec_simple_query()  //执行一个简单的查询语句
    pg_plan_queries()  //生成执行计划
      pg_plan_query()
        planner()
          standard_planner()
            subquery_planner()
              grouping_planner()
                query_planner()
                  add_base_rels_to_query()  //FROM子句
                    add_base_rels_to_query()  //FROM子句中的范围表
                      build_simple_rel()
                        get_relation_info()
                          heap_open()
                            relation_open()  //不加锁的方式打开表/视图等，因为之前已经
                            打开过，且不做修改
```

#5.第五，获取表上索引的信息，为此加AccessShareLock锁，原因如下：

```cpp
PostgresMain()
  exec_simple_query()  //执行一个简单的查询语句
    pg_plan_queries()
      pg_plan_query()
        planner()
          standard_planner()
            subquery_planner()
              grouping_planner()
                query_planner()
                  add_base_rels_to_query()
                    add_base_rels_to_query()
                      build_simple_rel()
                        get_relation_info()
                          index_open()  //打开索引
                            relation_open() //准备为索引加锁
                              LockRelationOid(relationId, lockmode=1=AccessShareLock)
                                     //此时为表/视图即RELATION加AccessShareLock锁
```

#第六，为获取约束信息在系统表上加锁：
conrel = heap_open(ConstraintRelationId, AccessShareLock);

#第七，为扫描系统表上加锁：

irel = index_open(indexId, AccessShareLock);

#8.第八，执行阶段，为RELATION加AccessShareLock锁：

```cpp
PostgresMain()
  exec_simple_query()
    PortalStart()
      ExecutorStart()  //执行器开始执行
        InitPlan()
          ExecInitNode()
            ExecInitSeqScan()
              InitScanRelation()
                ExecOpenScanRelation()
                  heap_open()
                    relation_open()
                      LockRelationOid(relationId, lockmode=1=AccessShareLock)
                      //此时为表/视图即RELATION加锁
```

