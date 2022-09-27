#1.ExecutorStart

```
ExecutorStart
--if (ExecutorStart_hook)
		(*ExecutorStart_hook) (queryDesc, eflags);
--else
		standard_ExecutorStart(queryDesc, eflags);
```

#2.standard_ExecutorStart

```
standard_ExecutorStart
--if ((XactReadOnly || IsInParallelMode()) &&
		!(eflags & EXEC_FLAG_EXPLAIN_ONLY))
----ExecCheckXactReadOnly(queryDesc->plannedstmt);
--estate = CreateExecutorState();
--queryDesc->estate = estate;
--switch (queryDesc->operation)
----case CMD_SELECT:
------if (queryDesc->plannedstmt->rowMarks != NIL ||
				queryDesc->plannedstmt->hasModifyingCTE)
--------estate->es_output_cid = GetCurrentCommandId(true);
----case CMD_INSERT:
----case CMD_DELETE:
----case CMD_UPDATE:
------estate->es_output_cid = GetCurrentCommandId(true);
--InitPlan(queryDesc, eflags);
```