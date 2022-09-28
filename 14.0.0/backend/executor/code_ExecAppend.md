#1.ExecAppend

```
ExecAppend
--if (!node->as_begun)
----if (node->as_nasyncplans > 0)
------ExecAppendAsyncBegin(node);
```