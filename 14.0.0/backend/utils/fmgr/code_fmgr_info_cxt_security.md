#1.fmgr_info_cxt_security

```
fmgr_info_cxt_security
--fmgr_isbuiltin
----index = fmgr_builtin_oid_index[id];//a mapper
----&fmgr_builtins[index];//返回数组，item，每个item是一个函数信息
--if ((fbp = fmgr_isbuiltin(functionId)) != NULL)
----finfo->fn_nargs = fbp->nargs;
		finfo->fn_strict = fbp->strict;
		finfo->fn_retset = fbp->retset;
		finfo->fn_stats = TRACK_FUNC_ALL;	/* ie, never track */
		finfo->fn_addr = fbp->func;
		finfo->fn_oid = functionId;
		return;
--else == NULL
----procedureTuple = SearchSysCache1(PROCOID, ObjectIdGetDatum(functionId));
------SearchCatCache1(SysCache[cacheId], key1);
--------SearchCatCacheInternal
```