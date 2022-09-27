#1.ExecShutdownNode

```
ExecShutdownNode
--planstate_tree_walker(node, ExecShutdownNode, NULL);
--switch (nodeTag(node))
	{
		case T_GatherState:
			ExecShutdownGather((GatherState *) node);
			break;
		case T_ForeignScanState:
			ExecShutdownForeignScan((ForeignScanState *) node);
			break;
		case T_CustomScanState:
			ExecShutdownCustomScan((CustomScanState *) node);
			break;
		case T_GatherMergeState:
			ExecShutdownGatherMerge((GatherMergeState *) node);
			break;
		case T_HashState:
			ExecShutdownHash((HashState *) node);
			break;
		case T_HashJoinState:
			ExecShutdownHashJoin((HashJoinState *) node);
			break;
		default:
			break;
	}
```