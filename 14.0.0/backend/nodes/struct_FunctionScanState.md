#1.struct FunctionScanState

```cpp
typedef struct FunctionScanState
{
	ScanState	ss;				/* its first field is NodeTag */
	int			eflags;
	bool		ordinality;
	bool		simple;
	int64		ordinal;
	int			nfuncs;
	struct FunctionScanPerFuncState *funcstates;	/* array of length nfuncs */
	MemoryContext argcontext;
} FunctionScanState;
```