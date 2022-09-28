#1. BitmapHeapNext

```
BitmapHeapNext
--tbm = node->tbm;
--if (pstate == NULL)
----tbmiterator = node->tbmiterator;
--else
----shared_tbmiterator = node->shared_tbmiterator;
--tbmres = node->tbmres;
--if (!node->initialized)
----if (!pstate)
------tbm = (TIDBitmap *) MultiExecProcNode(outerPlanState(node));
```