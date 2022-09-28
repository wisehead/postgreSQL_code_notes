#1.ExecInitAppend

```
ExecInitAppend
--appendstate->ps.plan = (Plan *) node;
--appendstate->ps.state = estate;
--appendstate->ps.ExecProcNode = ExecAppend;//!!!!!
--if (node->part_prune_info != NULL)
----prunestate = ExecCreatePartitionPruneState(&appendstate->ps,
												   node->part_prune_info);
----appendstate->as_prune_state = prunestate;
```