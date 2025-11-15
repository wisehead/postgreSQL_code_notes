#1.select

```cpp

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