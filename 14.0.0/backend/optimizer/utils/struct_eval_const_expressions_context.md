#1.struct eval_const_expressions_context

```cpp
typedef struct
{
	ParamListInfo boundParams;
	PlannerInfo *root;
	List	   *active_fns;
	Node	   *case_val;
	bool		estimate;
} eval_const_expressions_context;
```