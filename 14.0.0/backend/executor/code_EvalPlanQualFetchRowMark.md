#1.EvalPlanQualFetchRowMark

```
EvalPlanQualFetchRowMark
--if (erm->rti != erm->prti)
----datum = ExecGetJunkAttribute(epqstate->origslot,
									 earm->toidAttNo,
									 &isNull);
------slot_getattr(slot, attno, isNull);
--if (erm->markType == ROW_MARK_REFERENCE)
----ExecGetJunkAttribute
----if (erm->relation->rd_rel->relkind == RELKIND_FOREIGN_TABLE)
------fdwroutine = GetFdwRoutineForRelation(erm->relation, false);
--------if (relation->rd_fdwroutine == NULL)
----------fdwroutine = GetFdwRoutineByRelId(RelationGetRelid(relation));
--------else
---------return relation->rd_fdwroutine;
------fdwroutine->RefetchForeignRow(epqstate->recheckestate,
										  erm,
										  datum,
										  slot,
										  &updated);
------return true;							
----else
------table_tuple_fetch_row_version
--------rel->rd_tableam->tuple_fetch_row_version(rel, tid, snapshot, slot);
----------heapam_fetch_row_version
------------heap_fetch
--else
----ExecGetJunkAttribute
----ExecStoreHeapTupleDatum(datum, slot);		 
```