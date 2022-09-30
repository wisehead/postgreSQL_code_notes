#1.struct GroupState

```cpp
/* ---------------------
 *	GroupState information
 * ---------------------
 */
typedef struct GroupState
{
	ScanState	ss;				/* its first field is NodeTag */
	ExprState  *eqfunction;		/* equality function */
	bool		grp_done;		/* indicates completion of Group scan */
} GroupState;

```