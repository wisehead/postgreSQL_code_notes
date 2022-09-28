#1.ExecEndNode

```
ExecEndNode
--if (node->chgParam != NULL)
----bms_free(node->chgParam);
----node->chgParam = NULL;
--switch (nodeTag(node))
----case ....

```