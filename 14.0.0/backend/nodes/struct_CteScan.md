#1.struct CteScan

```cpp
/* ----------------
 *		CteScan node
 * ----------------
 */
typedef struct CteScan
{
	Scan		scan;
	int			ctePlanId;		/* ID of init SubPlan for CTE */
	int			cteParam;		/* ID of Param representing CTE output */
} CteScan;

```