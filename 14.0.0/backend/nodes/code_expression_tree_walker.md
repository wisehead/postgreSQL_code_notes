#1.expression_tree_walker

```
expression_tree_walker
--if (node == NULL)
----return false;
--check_stack_depth
--switch (nodeTag(node))
----case T_Var:
		case T_Const:
		case T_Param:
		case T_CaseTestExpr:
		case T_SQLValueFunction:
		case T_CoerceToDomainValue:
		case T_SetToDefault:
		case T_CurrentOfExpr:
		case T_NextValueExpr:
		case T_RangeTblRef:
		case T_SortGroupClause:
		case T_CTESearchClause:
			/* primitive node types with no expression subnodes */
------break;
----case.........
```