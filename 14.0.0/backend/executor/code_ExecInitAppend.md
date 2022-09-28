#1.ExecInitAppend

```
ExecInitAppend
--appendstate->ps.plan = (Plan *) node;
--appendstate->ps.state = estate;
--appendstate->ps.ExecProcNode = ExecAppend;//!!!!!
--if (node->part_prune_info != NULL)
----
```