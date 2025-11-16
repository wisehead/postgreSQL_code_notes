#1.GetSnapshotData

```
快照获取之上层，需要根据PostgreSQL的需要，为上层区分出不同类型的快照，如图9-1所示，包含有多种不同类型的快照调用了GetSnapshotData()函数。各种不同的快照类型，主要如下：· GetTransactionSnapshot()：在可重复性读或可串行化隔离级别下获取事务的快照。处理可串行化隔离级别时，需要调用GetSerializableTransactionSnapshotInt()。· GetLatestSnapshot()：获取最新的快照。· GetNonHistoricCatalogSnapshot()：获取系统表的最新的快照。· GetSerializableTransactionSnapshotInt()：在可串行化隔离级别下获取事务的快照。· SetTransactionSnapshot()：可以把数据库引擎之前保存的一个快照读入并设置为当前的快照。例如，获取系统元数据(system catalog)快照的调用栈如下，可以看到，需要读取系统表的时候，可以从系统表的快照来获取，以保持系统表数据获取的数据一致性。
```

```cpp
main()
  SubPostmasterMain()
    BackendRun()
      PostgresMain()
        exec_simple_query()
          pg_plan_queries()
            pg_plan_query()
              planner()
                standard_planner()
                  subquery_planner()
                    grouping_planner()
                      query_planner()
                        add_base_rels_to_query()
                          add_base_rels_to_query()
                            get_relation_info()
                              RelationGetFKeyList()
                                systable_beginscan() //需要扫描系统表
                                  index_open()
                                    relation_open()
                                      RelationIdGetRelation()
                                        RelationBuildDesc()
                                          ScanPgRelation()
                                            GetCatalogSnapshot()
                                            //获取系统元数据（system catalog）的快照
                                              GetNonHistoricCatalogSnapshot()
                                                GetSnapshotData()

```