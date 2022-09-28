#1.ExecInitBitmapOr

```
ExecInitBitmapOr
--nplans = list_length(node->bitmapplans);
--bitmaporstate->ps.ExecProcNode = ExecBitmapOr;
--foreach(l, node->bitmapplans)
----initNode = (Plan *) lfirst(l);
----bitmapplanstates[i] = ExecInitNode(initNode, estate, eflags);
----i++;
```