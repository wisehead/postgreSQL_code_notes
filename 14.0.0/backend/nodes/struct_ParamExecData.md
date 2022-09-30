#1.struct ParamExecData

```cpp
/* ----------------
 *	  ParamExecData
 *
 *	  ParamExecData entries are used for executor internal parameters
 *	  (that is, values being passed into or out of a sub-query).  The
 *	  paramid of a PARAM_EXEC Param is a (zero-based) index into an
 *	  array of ParamExecData records, which is referenced through
 *	  es_param_exec_vals or ecxt_param_exec_vals.
 *
 *	  If execPlan is not NULL, it points to a SubPlanState node that needs
 *	  to be executed to produce the value.  (This is done so that we can have
 *	  lazy evaluation of InitPlans: they aren't executed until/unless a
 *	  result value is needed.)	Otherwise the value is assumed to be valid
 *	  when needed.
 * ----------------
 */

typedef struct ParamExecData
{
	void	   *execPlan;		/* should be "SubPlanState *" */
	Datum		value;
	bool		isnull;
} ParamExecData;
```