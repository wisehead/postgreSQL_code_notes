#1.ExecSort

```
ExecSort
--tuplesortstate = (Tuplesortstate *) node->tuplesortstate;
--if (!node->sort_Done)
----Sort	   *plannode = (Sort *) node->ss.ps.plan;
----outerNode = outerPlanState(node);
----tupDesc = ExecGetResultType(outerNode);
----tuplesortstate = tuplesort_begin_heap
----tuplesort_set_bound
----for (;;)
------slot = ExecProcNode(outerNode);
------tuplesort_puttupleslot(tuplesortstate, slot);
----tuplesort_performsort(tuplesortstate);
--tuplesort_gettupleslot
```