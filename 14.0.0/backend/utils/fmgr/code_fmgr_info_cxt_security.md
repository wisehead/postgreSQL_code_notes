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
--//else == NULL
--procedureTuple = SearchSysCache1(PROCOID, ObjectIdGetDatum(functionId));
----SearchCatCache1(SysCache[cacheId], key1);
------SearchCatCacheInternal
--procedureStruct = (Form_pg_proc) GETSTRUCT(procedureTuple);
--switch (procedureStruct->prolang)
----case INTERNALlanguageId:
------prosrcdatum = SysCacheGetAttr(PROCOID, procedureTuple,
										  Anum_pg_proc_prosrc, &isnull);
------prosrc = TextDatumGetCString(prosrcdatum);
------fbp = fmgr_lookupByName(prosrc);
----case ClanguageId:
------fmgr_info_C_lang(functionId, finfo, procedureTuple);
---- case SQLlanguageId:
------finfo->fn_addr = fmgr_sql;
----default:
------fmgr_info_other_lang(functionId, finfo, procedureTuple);
--finfo->fn_oid = functionId;
--ReleaseSysCache(procedureTuple);
```