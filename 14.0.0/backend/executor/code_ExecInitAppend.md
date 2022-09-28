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
----if (prunestate->do_initial_prune)
------validsubplans = ExecFindInitialMatchingSubPlans
--------for (i = 0; i < prunestate->num_partprunedata; i++)
----------find_matching_subplans_recurse(prunedata, pprune, true, &result);
--else//ignore
--ExecInitResultTupleSlotTL(&appendstate->ps, &TTSOpsVirtual);
--while ((i = bms_next_member(validsubplans, i)) >= 0)
----Plan	   *initNode = (Plan *) list_nth(node->appendplans, i);
----appendplanstates[j++] = ExecInitNode(initNode, estate, eflags);
--appendstate->appendplans = appendplanstates;
```