#1.ExecMaterial

```
ExecMaterial
--tuplestorestate = node->tuplestorestate;
--if (tuplestorestate == NULL && node->eflags != 0)
----tuplestorestate = tuplestore_begin_heap(true, false, work_mem);
----tuplestore_set_eflags(tuplestorestate, node->eflags);
----tuplestore_alloc_read_pointer
----node->tuplestorestate = tuplestorestate;
--eof_tuplestore = (tuplestorestate == NULL) ||
		tuplestore_ateof(tuplestorestate);
--if (!forward && eof_tuplestore)
----if (!node->eof_underlying
------tuplestore_advance
--slot = node->ss.ps.ps_ResultTupleSlot;
--if (!eof_tuplestore)
----tuplestore_gettupleslot
--if (eof_tuplestore && !node->eof_underlying)
----outerNode = outerPlanState(node);
----outerslot = ExecProcNode(outerNode);
----tuplestore_puttupleslot
----ExecCopySlot(slot, outerslot);
----return slot;

```