#1.struct FdwRoutine

```cpp
/*
 * FdwRoutine is the struct returned by a foreign-data wrapper's handler
 * function.  It provides pointers to the callback functions needed by the
 * planner and executor.
 *
 * More function pointers are likely to be added in the future.  Therefore
 * it's recommended that the handler initialize the struct with
 * makeNode(FdwRoutine) so that all fields are set to NULL.  This will
 * ensure that no fields are accidentally left undefined.
 */
typedef struct FdwRoutine
{
	NodeTag		type;

	/* Functions for scanning foreign tables */
	GetForeignRelSize_function GetForeignRelSize;
	GetForeignPaths_function GetForeignPaths;
	GetForeignPlan_function GetForeignPlan;
	BeginForeignScan_function BeginForeignScan;
	IterateForeignScan_function IterateForeignScan;
	ReScanForeignScan_function ReScanForeignScan;
	EndForeignScan_function EndForeignScan;

	/*
	 * Remaining functions are optional.  Set the pointer to NULL for any that
	 * are not provided.
	 */

	/* Functions for remote-join planning */
	GetForeignJoinPaths_function GetForeignJoinPaths;

	/* Functions for remote upper-relation (post scan/join) planning */
	GetForeignUpperPaths_function GetForeignUpperPaths;

	/* Functions for updating foreign tables */
	AddForeignUpdateTargets_function AddForeignUpdateTargets;
	PlanForeignModify_function PlanForeignModify;
	BeginForeignModify_function BeginForeignModify;
	ExecForeignInsert_function ExecForeignInsert;
	ExecForeignBatchInsert_function ExecForeignBatchInsert;
	GetForeignModifyBatchSize_function GetForeignModifyBatchSize;
	ExecForeignUpdate_function ExecForeignUpdate;
	ExecForeignDelete_function ExecForeignDelete;
	EndForeignModify_function EndForeignModify;
	BeginForeignInsert_function BeginForeignInsert;
	EndForeignInsert_function EndForeignInsert;
	IsForeignRelUpdatable_function IsForeignRelUpdatable;
	PlanDirectModify_function PlanDirectModify;
	BeginDirectModify_function BeginDirectModify;
	IterateDirectModify_function IterateDirectModify;
	EndDirectModify_function EndDirectModify;

	/* Functions for SELECT FOR UPDATE/SHARE row locking */
	GetForeignRowMarkType_function GetForeignRowMarkType;
	RefetchForeignRow_function RefetchForeignRow;
	RecheckForeignScan_function RecheckForeignScan;

	/* Support functions for EXPLAIN */
	ExplainForeignScan_function ExplainForeignScan;
	ExplainForeignModify_function ExplainForeignModify;
	ExplainDirectModify_function ExplainDirectModify;

	/* Support functions for ANALYZE */
	AnalyzeForeignTable_function AnalyzeForeignTable;

	/* Support functions for IMPORT FOREIGN SCHEMA */
	ImportForeignSchema_function ImportForeignSchema;

	/* Support functions for TRUNCATE */
	ExecForeignTruncate_function ExecForeignTruncate;

	/* Support functions for parallelism under Gather node */
	IsForeignScanParallelSafe_function IsForeignScanParallelSafe;
	EstimateDSMForeignScan_function EstimateDSMForeignScan;
	InitializeDSMForeignScan_function InitializeDSMForeignScan;
	ReInitializeDSMForeignScan_function ReInitializeDSMForeignScan;
	InitializeWorkerForeignScan_function InitializeWorkerForeignScan;
	ShutdownForeignScan_function ShutdownForeignScan;

	/* Support functions for path reparameterization. */
	ReparameterizeForeignPathByChild_function ReparameterizeForeignPathByChild;

	/* Support functions for asynchronous execution */
	IsForeignPathAsyncCapable_function IsForeignPathAsyncCapable;
	ForeignAsyncRequest_function ForeignAsyncRequest;
	ForeignAsyncConfigureWait_function ForeignAsyncConfigureWait;
	ForeignAsyncNotify_function ForeignAsyncNotify;
} FdwRoutine;

```