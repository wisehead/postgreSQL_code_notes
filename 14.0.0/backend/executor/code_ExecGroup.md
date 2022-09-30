#1.ExecGroup

```
ExecGroup
--firsttupleslot = node->ss.ss_ScanTupleSlot;
--if (TupIsNull(firsttupleslot))
----outerslot = ExecProcNode(outerPlanState(node));
----ExecCopySlot(firsttupleslot, outerslot);
----econtext->ecxt_outertuple = firsttupleslot;
----if (ExecQual(node->ss.ps.qual, econtext))
------return ExecProject(node->ss.ps.ps_ProjInfo);
----else
------InstrCountFiltered1(node, 1);
--for (;;)
----for (;;)
------outerslot = ExecProcNode(outerPlanState(node));
------econtext->ecxt_innertuple = firsttupleslot;
------econtext->ecxt_outertuple = outerslot;
------if (!ExecQualAndReset(node->eqfunction, econtext))
--------break;
----ExecCopySlot(firsttupleslot, outerslot);
----econtext->ecxt_outertuple = firsttupleslot;
----if (ExecQual(node->ss.ps.qual, econtext))
------return ExecProject(node->ss.ps.ps_ProjInfo);
----else
------InstrCountFiltered1(node, 1);
```