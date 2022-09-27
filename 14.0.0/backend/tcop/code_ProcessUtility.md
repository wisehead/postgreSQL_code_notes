#1.ProcessUtility

```
ProcessUtility
--standard_ProcessUtility
```

#2.standard_ProcessUtility

```
standard_ProcessUtility
--readonly_flags = ClassifyUtilityCommandAsReadOnly(parsetree);
--if (readonly_flags != COMMAND_IS_STRICTLY_READ_ONLY &&
		(XactReadOnly || IsInParallelMode()))
----CommandTag	commandtag = CreateCommandTag(parsetree);
----if ((readonly_flags & COMMAND_OK_IN_READ_ONLY_TXN) == 0)
------PreventCommandIfReadOnly(GetCommandTagName(commandtag));
----if ((readonly_flags & COMMAND_OK_IN_PARALLEL_MODE) == 0)
------PreventCommandIfParallelMode(GetCommandTagName(commandtag));
----if ((readonly_flags & COMMAND_OK_IN_RECOVERY) == 0)
------PreventCommandDuringRecovery(GetCommandTagName(commandtag));
--pstate = make_parsestate(NULL);
--pstate->p_sourcetext = queryString;
--pstate->p_queryEnv = queryEnv;
--switch (nodeTag(parsetree))
----case ....
```