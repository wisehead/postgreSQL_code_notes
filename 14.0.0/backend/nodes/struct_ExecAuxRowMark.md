#1.struct ExecAuxRowMark

```cpp
/*
 * ExecAuxRowMark -
 *	   additional runtime representation of FOR [KEY] UPDATE/SHARE clauses
 *
 * Each LockRows and ModifyTable node keeps a list of the rowmarks it needs to
 * deal with.  In addition to a pointer to the related entry in es_rowmarks,
 * this struct carries the column number(s) of the resjunk columns associated
 * with the rowmark (see comments for PlanRowMark for more detail).
 */
typedef struct ExecAuxRowMark
{
	ExecRowMark *rowmark;		/* related entry in es_rowmarks */
	AttrNumber	ctidAttNo;		/* resno of ctid junk attribute, if any */
	AttrNumber	toidAttNo;		/* resno of tableoid junk attribute, if any */
	AttrNumber	wholeAttNo;		/* resno of whole-row junk attribute, if any */
} ExecAuxRowMark;
```