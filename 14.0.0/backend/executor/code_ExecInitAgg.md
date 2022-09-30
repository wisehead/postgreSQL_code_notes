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
----AggStatePerPhase phasedata = &aggstate->phases[++phase];
```