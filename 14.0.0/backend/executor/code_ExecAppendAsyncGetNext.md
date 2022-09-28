#1. ExecAppendAsyncGetNext

```
ExecAppendAsyncGetNext
--if (ExecAppendAsyncRequest(node, result))
----return true;
--while (node->as_nasyncremain > 0)
----ExecAppendAsyncEventWait(node);
----if (ExecAppendAsyncRequest(node, result))
------return true;
----if (!node->as_syncdone)
------break;
```