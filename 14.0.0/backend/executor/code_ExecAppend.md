#1.ExecAppend

```
ExecAppend
--if (!node->as_begun)
----if (node->as_nasyncplans > 0)
------ExecAppendAsyncBegin(node);
--for (;;)
----if (node->as_syncdone || !bms_is_empty(node->as_needrequest))
------(ExecAppendAsyncGetNext(node, &result))
----subnode = node->appendplans[node->as_whichplan];
----result = ExecProcNode(subnode);
----if (node->as_nasyncremain > 0)
------ExecAppendAsyncEventWait(node);
```