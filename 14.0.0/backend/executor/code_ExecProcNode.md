#1.ExecProcNode

```
ExecProcNode
--if (node->chgParam != NULL) /* something changed? */
----ExecReScan(node);		/* let ReScan handle this */
--return node->ExecProcNode(node);
```