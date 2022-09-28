#1.struct BitmapIndexScanState

```cpp
/* ----------------
 *	 BitmapIndexScanState information
 *
 *		result			   bitmap to return output into, or NULL
 *		ScanKeys		   Skey structures for index quals
 *		NumScanKeys		   number of ScanKeys
 *		RuntimeKeys		   info about Skeys that must be evaluated at runtime
 *		NumRuntimeKeys	   number of RuntimeKeys
 *		ArrayKeys		   info about Skeys that come from ScalarArrayOpExprs
 *		NumArrayKeys	   number of ArrayKeys
 *		RuntimeKeysReady   true if runtime Skeys have been computed
 *		RuntimeContext	   expr context for evaling runtime Skeys
 *		RelationDesc	   index relation descriptor
 *		ScanDesc		   index scan descriptor
 * ----------------
 */
typedef struct BitmapIndexScanState
{
	ScanState	ss;				/* its first field is NodeTag */
	TIDBitmap  *biss_result;
	struct ScanKeyData *biss_ScanKeys;
	int			biss_NumScanKeys;
	IndexRuntimeKeyInfo *biss_RuntimeKeys;
	int			biss_NumRuntimeKeys;
	IndexArrayKeyInfo *biss_ArrayKeys;
	int			biss_NumArrayKeys;
	bool		biss_RuntimeKeysReady;
	ExprContext *biss_RuntimeContext;
	Relation	biss_RelationDesc;
	struct IndexScanDescData *biss_ScanDesc;
} BitmapIndexScanState;
```