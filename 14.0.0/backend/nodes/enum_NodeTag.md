#1.enum NodeTag

```cpp
/*
 * The first field of every node is NodeTag. Each node created (with makeNode)
 * will have one of the following tags as the value of its first field.
 *
 * Note that inserting or deleting node types changes the numbers of other
 * node types later in the list.  This is no problem during development, since
 * the node numbers are never stored on disk.  But don't do it in a released
 * branch, because that would represent an ABI break for extensions.
 */
typedef enum NodeTag
{
	T_Invalid = 0,

	/*
	 * TAGS FOR EXECUTOR NODES (execnodes.h)
	 */
	T_IndexInfo,
	T_ExprContext,
	T_ProjectionInfo,
	T_JunkFilter,
	T_OnConflictSetState,
	T_ResultRelInfo,
	T_EState,
	T_TupleTableSlot,

	/*
	 * TAGS FOR PLAN NODES (plannodes.h)
	 */
	T_Plan,
	T_Result,
	T_ProjectSet,
	T_ModifyTable,
	T_Append,
	T_MergeAppend,
	T_RecursiveUnion,
	T_BitmapAnd,
	T_BitmapOr,
	T_Scan,
	T_SeqScan,
	T_SampleScan,
	T_IndexScan,
	T_IndexOnlyScan,
	T_BitmapIndexScan,
	T_BitmapHeapScan,
	T_TidScan,
	T_TidRangeScan,
	T_SubqueryScan,
	T_FunctionScan,
	T_ValuesScan,
	T_TableFuncScan,
	T_CteScan,
	T_NamedTuplestoreScan,
	T_WorkTableScan,
	T_ForeignScan,
	T_CustomScan,
	T_Join,
	T_NestLoop,
	T_MergeJoin,
	T_HashJoin,
	T_Material,
	T_Memoize,
	T_Sort,
	T_IncrementalSort,
	T_Group,
	T_Agg,
	T_WindowAgg,
	T_Unique,
	T_Gather,
	T_GatherMerge,
	T_Hash,
	T_SetOp,
	T_LockRows,
	T_Limit,
	/* these aren't subclasses of Plan: */
	T_NestLoopParam,
	T_PlanRowMark,
	T_PartitionPruneInfo,
	T_PartitionedRelPruneInfo,
	T_PartitionPruneStepOp,
	T_PartitionPruneStepCombine,
	T_PlanInvalItem,

	/*
	 * TAGS FOR PLAN STATE NODES (execnodes.h)
	 *
	 * These should correspond one-to-one with Plan node types.
	 */
	T_PlanState,
	T_ResultState,
	T_ProjectSetState,
	T_ModifyTableState,
	T_AppendState,
	T_MergeAppendState,
	T_RecursiveUnionState,
	T_BitmapAndState,
	T_BitmapOrState,
	T_ScanState,
	T_SeqScanState,
	T_SampleScanState,
	T_IndexScanState,
	T_IndexOnlyScanState,
	T_BitmapIndexScanState,
	T_BitmapHeapScanState,
	T_TidScanState,
	T_TidRangeScanState,
	T_SubqueryScanState,
	T_FunctionScanState,
	T_TableFuncScanState,
	T_ValuesScanState,
	T_CteScanState,
	T_NamedTuplestoreScanState,
	T_WorkTableScanState,
	T_ForeignScanState,
	T_CustomScanState,
	T_JoinState,
	T_NestLoopState,
	T_MergeJoinState,
	T_HashJoinState,
	T_MaterialState,
	T_MemoizeState,
	T_SortState,
	T_IncrementalSortState,
	T_GroupState,
	T_AggState,
	T_WindowAggState,
	T_UniqueState,
	T_GatherState,
	T_GatherMergeState,
	T_HashState,
	T_SetOpState,
	T_LockRowsState,
	T_LimitState,

	/*
	 * TAGS FOR PRIMITIVE NODES (primnodes.h)
	 */
	T_Alias,
	T_RangeVar,
	T_TableFunc,
	T_Expr,
	T_Var,
	T_Const,
	T_Param,
	T_Aggref,
	T_GroupingFunc,
	T_WindowFunc,
	T_SubscriptingRef,
	T_FuncExpr,
	T_NamedArgExpr,
	T_OpExpr,
	T_DistinctExpr,
	T_NullIfExpr,
	T_ScalarArrayOpExpr,
	T_BoolExpr,
	T_SubLink,
	T_SubPlan,
	T_AlternativeSubPlan,
	T_FieldSelect,
	T_FieldStore,
	T_RelabelType,
	T_CoerceViaIO,
	T_ArrayCoerceExpr,
	T_ConvertRowtypeExpr,
	T_CollateExpr,
	T_CaseExpr,
	T_CaseWhen,
	T_CaseTestExpr,
	T_ArrayExpr,
	T_RowExpr,
	T_RowCompareExpr,
	T_CoalesceExpr,
	T_MinMaxExpr,
	T_SQLValueFunction,
	T_XmlExpr,
	T_NullTest,
	T_BooleanTest,
	T_CoerceToDomain,
	T_CoerceToDomainValue,
	T_SetToDefault,
	T_CurrentOfExpr,
	T_NextValueExpr,
	T_InferenceElem,
	T_TargetEntry,
	T_RangeTblRef,
	T_JoinExpr,
	T_FromExpr,
	T_OnConflictExpr,
	T_IntoClause,

	/*
	 * TAGS FOR EXPRESSION STATE NODES (execnodes.h)
	 *
	 * ExprState represents the evaluation state for a whole expression tree.
	 * Most Expr-based plan nodes do not have a corresponding expression state
	 * node, they're fully handled within execExpr* - but sometimes the state
	 * needs to be shared with other parts of the executor, as for example
	 * with SubPlanState, which nodeSubplan.c has to modify.
	 */
	T_ExprState,
	T_WindowFuncExprState,
	T_SetExprState,
	T_SubPlanState,
	T_DomainConstraintState,

	/*
	 * TAGS FOR PLANNER NODES (pathnodes.h)
	 */
	T_PlannerInfo,
	T_PlannerGlobal,
	T_RelOptInfo,
	T_IndexOptInfo,
	T_ForeignKeyOptInfo,
	T_ParamPathInfo,
	T_Path,
	T_IndexPath,
	T_BitmapHeapPath,
	T_BitmapAndPath,
	T_BitmapOrPath,
	T_TidPath,
	T_TidRangePath,
	T_SubqueryScanPath,
	T_ForeignPath,
	T_CustomPath,
	T_NestPath,
	T_MergePath,
	T_HashPath,
	T_AppendPath,
	T_MergeAppendPath,
	T_GroupResultPath,
	T_MaterialPath,
	T_MemoizePath,
	T_UniquePath,
	T_GatherPath,
	T_GatherMergePath,
	T_ProjectionPath,
	T_ProjectSetPath,
	T_SortPath,
	T_IncrementalSortPath,
	T_GroupPath,
	T_UpperUniquePath,
	T_AggPath,
	T_GroupingSetsPath,
	T_MinMaxAggPath,
	T_WindowAggPath,
	T_SetOpPath,
	T_RecursiveUnionPath,
	T_LockRowsPath,
	T_ModifyTablePath,
	T_LimitPath,
	/* these aren't subclasses of Path: */
	T_EquivalenceClass,
	T_EquivalenceMember,
	T_PathKey,
	T_PathTarget,
	T_RestrictInfo,
	T_IndexClause,
	T_PlaceHolderVar,
	T_SpecialJoinInfo,
	T_AppendRelInfo,
	T_RowIdentityVarInfo,
	T_PlaceHolderInfo,
	T_MinMaxAggInfo,
	T_PlannerParamItem,
	T_RollupData,
	T_GroupingSetData,
	T_StatisticExtInfo,

	/*
	 * TAGS FOR MEMORY NODES (memnodes.h)
	 */
	T_MemoryContext,
	T_AllocSetContext,
	T_SlabContext,
	T_GenerationContext,

	/*
	 * TAGS FOR VALUE NODES (value.h)
	 */
	T_Value,
	T_Integer,
	T_Float,
	T_String,
	T_BitString,
	T_Null,

	/*
	 * TAGS FOR LIST NODES (pg_list.h)
	 */
	T_List,
	T_IntList,
	T_OidList,

	/*
	 * TAGS FOR EXTENSIBLE NODES (extensible.h)
	 */
	T_ExtensibleNode,

	/*
	 * TAGS FOR STATEMENT NODES (mostly in parsenodes.h)
	 */
	T_RawStmt,
	T_Query,
	T_PlannedStmt,
	T_InsertStmt,
	T_DeleteStmt,
	T_UpdateStmt,
	T_SelectStmt,
	T_ReturnStmt,
	T_PLAssignStmt,
	T_AlterTableStmt,
	T_AlterTableCmd,
	T_AlterDomainStmt,
	T_SetOperationStmt,
	T_GrantStmt,
	T_GrantRoleStmt,
	T_AlterDefaultPrivilegesStmt,
	T_ClosePortalStmt,
	T_ClusterStmt,
	T_CopyStmt,
	T_CreateStmt,
	T_DefineStmt,
	T_DropStmt,
	T_TruncateStmt,
	T_CommentStmt,
	T_FetchStmt,
	T_IndexStmt,
	T_CreateFunctionStmt,
	T_AlterFunctionStmt,
	T_DoStmt,
	T_RenameStmt,
	T_RuleStmt,
	T_NotifyStmt,
	T_ListenStmt,
	T_UnlistenStmt,
	T_TransactionStmt,
	T_ViewStmt,
	T_LoadStmt,
	T_CreateDomainStmt,
	T_CreatedbStmt,
	T_DropdbStmt,
	T_VacuumStmt,
	T_ExplainStmt,
	T_CreateTableAsStmt,
	T_CreateSeqStmt,
	T_AlterSeqStmt,
	T_VariableSetStmt,
	T_VariableShowStmt,
	T_DiscardStmt,
	T_CreateTrigStmt,
	T_CreatePLangStmt,
	T_CreateRoleStmt,
	T_AlterRoleStmt,
	T_DropRoleStmt,
	T_LockStmt,
	T_ConstraintsSetStmt,
	T_ReindexStmt,
	T_CheckPointStmt,
	T_CreateSchemaStmt,
	T_AlterDatabaseStmt,
	T_AlterDatabaseSetStmt,
	T_AlterRoleSetStmt,
	T_CreateConversionStmt,
	T_CreateCastStmt,
	T_CreateOpClassStmt,
	T_CreateOpFamilyStmt,
	T_AlterOpFamilyStmt,
	T_PrepareStmt,
	T_ExecuteStmt,
	T_DeallocateStmt,
	T_DeclareCursorStmt,
	T_CreateTableSpaceStmt,
	T_DropTableSpaceStmt,
	T_AlterObjectDependsStmt,
	T_AlterObjectSchemaStmt,
	T_AlterOwnerStmt,
	T_AlterOperatorStmt,
	T_AlterTypeStmt,
	T_DropOwnedStmt,
	T_ReassignOwnedStmt,
	T_CompositeTypeStmt,
	T_CreateEnumStmt,
	T_CreateRangeStmt,
	T_AlterEnumStmt,
	T_AlterTSDictionaryStmt,
	T_AlterTSConfigurationStmt,
	T_CreateFdwStmt,
	T_AlterFdwStmt,
	T_CreateForeignServerStmt,
	T_AlterForeignServerStmt,
	T_CreateUserMappingStmt,
	T_AlterUserMappingStmt,
	T_DropUserMappingStmt,
	T_AlterTableSpaceOptionsStmt,
	T_AlterTableMoveAllStmt,
	T_SecLabelStmt,
	T_CreateForeignTableStmt,
	T_ImportForeignSchemaStmt,
	T_CreateExtensionStmt,
	T_AlterExtensionStmt,
	T_AlterExtensionContentsStmt,
	T_CreateEventTrigStmt,
	T_AlterEventTrigStmt,
	T_RefreshMatViewStmt,
	T_ReplicaIdentityStmt,
	T_AlterSystemStmt,
	T_CreatePolicyStmt,
	T_AlterPolicyStmt,
	T_CreateTransformStmt,
	T_CreateAmStmt,
	T_CreatePublicationStmt,
	T_AlterPublicationStmt,
	T_CreateSubscriptionStmt,
	T_AlterSubscriptionStmt,
	T_DropSubscriptionStmt,
	T_CreateStatsStmt,
	T_AlterCollationStmt,
	T_CallStmt,
	T_AlterStatsStmt,

	/*
	 * TAGS FOR PARSE TREE NODES (parsenodes.h)
	 */
	T_A_Expr,
	T_ColumnRef,
	T_ParamRef,
	T_A_Const,
	T_FuncCall,
	T_A_Star,
	T_A_Indices,
	T_A_Indirection,
	T_A_ArrayExpr,
	T_ResTarget,
	T_MultiAssignRef,
	T_TypeCast,
	T_CollateClause,
	T_SortBy,
	T_WindowDef,
	T_RangeSubselect,
	T_RangeFunction,
	T_RangeTableSample,
	T_RangeTableFunc,
	T_RangeTableFuncCol,
	T_TypeName,
	T_ColumnDef,
	T_IndexElem,
	T_StatsElem,
	T_Constraint,
	T_DefElem,
	T_RangeTblEntry,
	T_RangeTblFunction,
	T_TableSampleClause,
	T_WithCheckOption,
	T_SortGroupClause,
	T_GroupingSet,
	T_WindowClause,
	T_ObjectWithArgs,
	T_AccessPriv,
	T_CreateOpClassItem,
	T_TableLikeClause,
	T_FunctionParameter,
	T_LockingClause,
	T_RowMarkClause,
	T_XmlSerialize,
	T_WithClause,
	T_InferClause,
	T_OnConflictClause,
	T_CTESearchClause,
	T_CTECycleClause,
	T_CommonTableExpr,
	T_RoleSpec,
	T_TriggerTransition,
	T_PartitionElem,
	T_PartitionSpec,
	T_PartitionBoundSpec,
	T_PartitionRangeDatum,
	T_PartitionCmd,
	T_VacuumRelation,

	/*
	 * TAGS FOR REPLICATION GRAMMAR PARSE NODES (replnodes.h)
	 */
	T_IdentifySystemCmd,
	T_BaseBackupCmd,
	T_CreateReplicationSlotCmd,
	T_DropReplicationSlotCmd,
	T_StartReplicationCmd,
	T_TimeLineHistoryCmd,
	T_SQLCmd,

	/*
	 * TAGS FOR RANDOM OTHER STUFF
	 *
	 * These are objects that aren't part of parse/plan/execute node tree
	 * structures, but we give them NodeTags anyway for identification
	 * purposes (usually because they are involved in APIs where we want to
	 * pass multiple object types through the same pointer).
	 */
	T_TriggerData,				/* in commands/trigger.h */
	T_EventTriggerData,			/* in commands/event_trigger.h */
	T_ReturnSetInfo,			/* in nodes/execnodes.h */
	T_WindowObjectData,			/* private in nodeWindowAgg.c */
	T_TIDBitmap,				/* in nodes/tidbitmap.h */
	T_InlineCodeBlock,			/* in nodes/parsenodes.h */
	T_FdwRoutine,				/* in foreign/fdwapi.h */
	T_IndexAmRoutine,			/* in access/amapi.h */
	T_TableAmRoutine,			/* in access/tableam.h */
	T_TsmRoutine,				/* in access/tsmapi.h */
	T_ForeignKeyCacheInfo,		/* in utils/rel.h */
	T_CallContext,				/* in nodes/parsenodes.h */
	T_SupportRequestSimplify,	/* in nodes/supportnodes.h */
	T_SupportRequestSelectivity,	/* in nodes/supportnodes.h */
	T_SupportRequestCost,		/* in nodes/supportnodes.h */
	T_SupportRequestRows,		/* in nodes/supportnodes.h */
	T_SupportRequestIndexCondition	/* in nodes/supportnodes.h */
} NodeTag;

```