#1. ExecCreatePartitionPruneState

```
ExecCreatePartitionPruneState
--estate->es_partition_directory =
			CreatePartitionDirectory(estate->es_query_cxt, false);
--n_part_hierarchies = list_length(partitionpruneinfo->prune_infos);
--prunestate->other_subplans = bms_copy(partitionpruneinfo->other_subplans);
--foreach(lc, partitionpruneinfo->prune_infos)
----List	   *partrelpruneinfos = lfirst_node(List, lc);
----prunestate->partprunedata[i] = prunedata;
----prunedata->num_partrelprunedata = npartrelpruneinfos;
----foreach(lc2, partrelpruneinfos)
------PartitionedRelPruneInfo *pinfo = lfirst_node(PartitionedRelPruneInfo, lc2);
------PartitionedRelPruningData *pprune = &prunedata->partrelprunedata[j];
------partrel = ExecGetRangeTableRelation(estate, pinfo->rtindex);
------partkey = RelationGetPartitionKey(partrel);
------partdesc = PartitionDirectoryLookup
------pprune->nparts = partdesc->nparts;
------pprune->subplan_map = palloc(sizeof(int) * partdesc->nparts);
------if (partdesc->nparts == pinfo->nparts)
--------pprune->subpart_map = pinfo->subpart_map;
--------memcpy(pprune->subplan_map, pinfo->subplan_map,
					   sizeof(int) * pinfo->nparts);
------else
--------//todo
------pprune->present_parts = bms_copy(pinfo->present_parts);
------if (pinfo->initial_pruning_steps)
--------ExecInitPruningContext(&pprune->initial_context,
									   pinfo->initial_pruning_steps,
									   partdesc, partkey, planstate);
------if (pinfo->exec_pruning_steps)
--------ExecInitPruningContext(&pprune->exec_context,
									   pinfo->exec_pruning_steps,
									   partdesc, partkey, planstate);
------prunestate->execparamids = bms_add_members(prunestate->execparamids,
									pinfo->execparamids);
--return prunestate;
```

#2.caller

```
ExecInitAppend
----prunestate = ExecCreatePartitionPruneState(&appendstate->ps,
												   node->part_prune_info);
----appendstate->as_prune_state = prunestate;
```