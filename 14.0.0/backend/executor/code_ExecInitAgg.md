#1.ExecInitAgg

```
ExecInitAgg
--aggstate->ss.ps.ExecProcNode = ExecAgg;
--if (node->groupingSets)
----numGroupingSets = list_length(node->groupingSets);
--aggstate->maxsets = numGroupingSets;
--aggstate->numphases = numPhases;
--outerPlan = outerPlan(node);
--outerPlanState(aggstate) = ExecInitNode(outerPlan, estate, eflags);
--aggstate->ss.ps.outerops =
		ExecGetResultSlotOps(outerPlanState(&aggstate->ss),
							 &aggstate->ss.ps.outeropsfixed);
--ExecCreateScanSlotFromOuterPlan(estate, &aggstate->ss,
									aggstate->ss.ps.outerops);
--scanDesc = aggstate->ss.ss_ScanTupleSlot->tts_tupleDescriptor;
--if (numPhases > 2)
----aggstate->sort_slot = ExecInitExtraTupleSlot
--ExecInitResultTupleSlotTL(&aggstate->ss.ps, &TTSOpsVirtual);
--ExecAssignProjectionInfo(&aggstate->ss.ps, NULL);
--aggstate->ss.ps.qual =
		ExecInitQual(node->plan.qual, (PlanState *) aggstate);
--for (phaseidx = 0; phaseidx <= list_length(node->chain); ++phaseidx)
----aggnode = list_nth_node(Agg, node->chain, phaseidx - 1);
----sortnode = castNode(Sort, aggnode->plan.lefttree);
----if (aggnode->aggstrategy == AGG_HASHED
			|| aggnode->aggstrategy == AGG_MIXED)
------//todo
----else
------AggStatePerPhase phasedata = &aggstate->phases[++phase];
------phasedata->numsets = num_sets = list_length(aggnode->groupingSets);
------if (num_sets)
--------foreach(l, aggnode->groupingSets)
----------for (j = 0; j < current_length; ++j)
------------cols = bms_add_member(cols, aggnode->grpColIdx[j]);
----------phasedata->grouped_cols[i] = cols;
--------all_grouped_cols = bms_add_members(all_grouped_cols,
												   phasedata->grouped_cols[0]);
------if (aggnode->aggstrategy == AGG_SORTED)
--------for (i = 0; i < phasedata->numsets; i++)
----------phasedata->eqfunctions[length - 1] =
						execTuplesMatchPrepare(scanDesc,
											   length,
											   aggnode->grpColIdx,
											   aggnode->grpOperators,
											   aggnode->grpCollations,
											   (PlanState *) aggstate);
```