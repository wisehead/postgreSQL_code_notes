#1.struct WorkTableScanState

```cpp
/* ----------------
 *	 WorkTableScanState information
 *
 *		WorkTableScan nodes are used to scan the work table created by
 *		a RecursiveUnion node.  We locate the RecursiveUnion node
 *		during executor startup.
 * ----------------
 */
typedef struct WorkTableScanState
{
	ScanState	ss;				/* its first field is NodeTag */
	RecursiveUnionState *rustate;
} WorkTableScanState;
```