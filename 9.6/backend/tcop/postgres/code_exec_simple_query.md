#1.exec_simple_query

```cpp
exec_simple_query
--start_xact_command
--pg_parse_query
--foreach(parsetree_item, parsetree_list)
----commandTag = CreateCommandTag(parsetree);
----BeginCommand(commandTag, dest);
----start_xact_command();
----pg_analyze_and_rewrite
----pg_plan_queries
----CreatePortal
----PortalDefineQuery
----PortalStart
----CreateDestReceiver
----PortalRun
----PortalDrop
----finish_xact_command
----EndCommand
--finish_xact_command
--

```
