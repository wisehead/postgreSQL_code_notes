#1.struct SeqScanState

```cpp
/* ----------------
 *	 SeqScanState information
 * ----------------
 */
typedef struct SeqScanState
{
	ScanState	ss;				/* its first field is NodeTag */
	Size		pscan_len;		/* size of parallel heap scan descriptor */
} SeqScanState;
```