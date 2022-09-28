#1.ExecInitBitmapAnd

```
ExecInitBitmapAnd
--nplans = list_length(node->bitmapplans);
--bitmapandstate->ps.ExecProcNode = ExecBitmapAnd;
--foreach(l, node->bitmapplans)
----initNode = (Plan *) lfirst(l);
----bitmapplanstates[i] = ExecInitNode(initNode, estate, eflags);
----i++;
```