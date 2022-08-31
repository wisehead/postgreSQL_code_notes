#1.enum NodeTag

```cpp
/*
 * The first field of every node is NodeTag. Each node created (with makeNode)
 * will have one of the following tags as the value of its first field.
 *
 * Note that the numbers of the node tags are not contiguous. We left holes
 * here so that we can add more tags without changing the existing enum's.
 * (Since node tag numbers never exist outside backend memory, there's no
 * real harm in renumbering, it just costs a full rebuild ...)
 */
typedef enum NodeTag
{
    T_Invalid = 0,

    /*
     * TAGS FOR EXECUTOR NODES (execnodes.h)
     */
    T_IndexInfo = 10,
    T_ExprContext,
    T_ProjectionInfo,
    T_JunkFilter,
    T_ResultRelInfo,
    T_EState,
    T_TupleTableSlot,

    /*
     * TAGS FOR PLAN NODES (plannodes.h)
     */
    T_Plan = 100,
    T_Result,
    T_Append,
    T_RecursiveUnion,
    T_BitmapAnd,
    T_BitmapOr,
    T_Scan,
    T_SeqScan,
    T_IndexScan,
    T_BitmapIndexScan,
    T_BitmapHeapScan,
    T_TidScan,
    T_SubqueryScan,
    T_FunctionScan,
    T_ValuesScan,
    T_CteScan,
    T_WorkTableScan,
    T_Join,
    T_NestLoop,
    T_MergeJoin,
    T_HashJoin,
    T_Material,
    T_Sort,
    T_Group,
    T_Agg,
    T_WindowAgg,
    T_Unique,
    T_Hash,
    T_SetOp,
    T_Limit,
    /* this one isn't a subclass of Plan: */
    T_PlanInvalItem,

    /*
     * TAGS FOR PLAN STATE NODES (execnodes.h)
     *
     * These should correspond one-to-one with Plan node types.
     */
    T_PlanState = 200,
    T_ResultState,
    T_AppendState,
    T_RecursiveUnionState,
    T_BitmapAndState,
    T_BitmapOrState,
    T_ScanState,
    T_SeqScanState,
    T_IndexScanState,
    T_BitmapIndexScanState,
    T_BitmapHeapScanState,
    T_TidScanState,
    T_SubqueryScanState,
    T_FunctionScanState,
    T_ValuesScanState,
    T_CteScanState,
    T_WorkTableScanState,
    T_JoinState,
    T_NestLoopState,
    T_MergeJoinState,
    T_HashJoinState,
    T_MaterialState,
    T_SortState,
    T_GroupState,
    T_AggState,
    T_WindowAggState,
    T_UniqueState,
    T_HashState,
    T_SetOpState,
    T_LimitState,

    /*
     * TAGS FOR PRIMITIVE NODES (primnodes.h)
     */
    T_Alias = 300,
    T_RangeVar,
    T_Expr,
    T_Var,
    T_Const,
    T_Param,
    T_Aggref,
    T_WindowFunc,
    T_ArrayRef,
    T_FuncExpr,
    T_OpExpr,
    T_DistinctExpr,
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
    T_CaseExpr,
    T_CaseWhen,
    T_CaseTestExpr,
    T_ArrayExpr,
    T_RowExpr,
    T_RowCompareExpr,
    T_CoalesceExpr,
    T_MinMaxExpr,
    T_XmlExpr,
    T_NullIfExpr,
    T_NullTest,
    T_BooleanTest,
    T_CoerceToDomain,
    T_CoerceToDomainValue,
    T_SetToDefault,
    T_CurrentOfExpr,
    T_TargetEntry,
    T_RangeTblRef,
    T_JoinExpr,
    T_FromExpr,
    T_IntoClause,

    /*
     * TAGS FOR EXPRESSION STATE NODES (execnodes.h)
     *
     * These correspond (not always one-for-one) to primitive nodes derived
     * from Expr.
     */
    T_ExprState = 400,
    T_GenericExprState,
    T_AggrefExprState,
    T_WindowFuncExprState,
    T_ArrayRefExprState,
    T_FuncExprState,
    T_ScalarArrayOpExprState,
    T_BoolExprState,
    T_SubPlanState,
    T_AlternativeSubPlanState,
    T_FieldSelectState,
    T_FieldStoreState,
    T_CoerceViaIOState,
    T_ArrayCoerceExprState,
    T_ConvertRowtypeExprState,
    T_CaseExprState,
    T_CaseWhenState,
    T_ArrayExprState,
    T_RowExprState,
    T_RowCompareExprState,
    T_CoalesceExprState,
    T_MinMaxExprState,
    T_XmlExprState,
    T_NullTestState,
    T_CoerceToDomainState,
    T_DomainConstraintState,
    T_WholeRowVarExprState,     /* will be in a more natural position in 9.3 */

    /*
     * TAGS FOR PLANNER NODES (relation.h)
     */
    T_PlannerInfo = 500,
    T_PlannerGlobal,
    T_RelOptInfo,
    T_IndexOptInfo,
    T_Path,
    T_IndexPath,
    T_BitmapHeapPath,
    T_BitmapAndPath,
    T_BitmapOrPath,
    T_NestPath,
    T_MergePath,
    T_HashPath,
    T_TidPath,
    T_AppendPath,
    T_ResultPath,
    T_MaterialPath,
    T_UniquePath,
    T_EquivalenceClass,
    T_EquivalenceMember,
    T_PathKey,
    T_RestrictInfo,
    T_InnerIndexscanInfo,
    T_PlaceHolderVar,
    T_SpecialJoinInfo,
    T_AppendRelInfo,
    T_PlaceHolderInfo,
    T_PlannerParamItem,

    /*
     * TAGS FOR MEMORY NODES (memnodes.h)
     */
    T_MemoryContext = 600,
    T_AllocSetContext,

    /*
     * TAGS FOR VALUE NODES (value.h)
     */
    T_Value = 650,
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
     * TAGS FOR STATEMENT NODES (mostly in parsenodes.h)
     */
    T_Query = 700,
    T_PlannedStmt,
    T_InsertStmt,
    T_DeleteStmt,
    T_UpdateStmt,
    T_SelectStmt,
    T_AlterTableStmt,
    T_AlterTableCmd,
    T_AlterDomainStmt,
    T_SetOperationStmt,
    T_GrantStmt,
    T_GrantRoleStmt,
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
    T_RemoveFuncStmt,
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
    T_CreateSeqStmt,
    T_AlterSeqStmt,
    T_VariableSetStmt,
    T_VariableShowStmt,
    T_DiscardStmt,
    T_CreateTrigStmt,
    T_DropPropertyStmt,
    T_CreatePLangStmt,
    T_DropPLangStmt,
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
    T_DropCastStmt,
    T_CreateOpClassStmt,
    T_CreateOpFamilyStmt,
    T_AlterOpFamilyStmt,
    T_RemoveOpClassStmt,
    T_RemoveOpFamilyStmt,
    T_PrepareStmt,
    T_ExecuteStmt,
    T_DeallocateStmt,
    T_DeclareCursorStmt,
    T_CreateTableSpaceStmt,
    T_DropTableSpaceStmt,
    T_AlterObjectSchemaStmt,
    T_AlterOwnerStmt,
    T_DropOwnedStmt,
    T_ReassignOwnedStmt,
    T_CompositeTypeStmt,
    T_CreateEnumStmt,
    T_AlterTSDictionaryStmt,
    T_AlterTSConfigurationStmt,
    T_CreateFdwStmt,
    T_AlterFdwStmt,
    T_DropFdwStmt,
    T_CreateForeignServerStmt,
    T_AlterForeignServerStmt,
    T_DropForeignServerStmt,
    T_CreateUserMappingStmt,
    T_AlterUserMappingStmt,
    T_DropUserMappingStmt,

    /*
     * TAGS FOR PARSE TREE NODES (parsenodes.h)
     */
    T_A_Expr = 900,
    T_ColumnRef,
    T_ParamRef,
    T_A_Const,
    T_FuncCall,
    T_A_Star,
    T_A_Indices,
    T_A_Indirection,
    T_A_ArrayExpr,
    T_ResTarget,
    T_TypeCast,
    T_SortBy,
    T_WindowDef,
    T_RangeSubselect,
    T_RangeFunction,
    T_TypeName,
    T_ColumnDef,
    T_IndexElem,
    T_Constraint,
    T_DefElem,
    T_RangeTblEntry,
    T_SortGroupClause,
    T_WindowClause,
    T_FkConstraint,
    T_PrivGrantee,
    T_FuncWithArgs,
    T_AccessPriv,
    T_CreateOpClassItem,
    T_InhRelation,
    T_FunctionParameter,
    T_LockingClause,
    T_RowMarkClause,
    T_XmlSerialize,
    T_WithClause,
    T_CommonTableExpr,

    /*
     * TAGS FOR RANDOM OTHER STUFF
     *
     * These are objects that aren't part of parse/plan/execute node tree
     * structures, but we give them NodeTags anyway for identification
     * purposes (usually because they are involved in APIs where we want to
     * pass multiple object types through the same pointer).
     */
    T_TriggerData = 950,        /* in commands/trigger.h */
    T_ReturnSetInfo,            /* in nodes/execnodes.h */
    T_WindowObjectData,         /* private in nodeWindowAgg.c */
    T_TIDBitmap                 /* in nodes/tidbitmap.h */
} NodeTag;                         
```