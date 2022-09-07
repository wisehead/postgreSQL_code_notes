#1.PrepareSortSupportFromOrderingOp

```
PrepareSortSupportFromOrderingOp
--get_ordering_op_properties
----SearchSysCacheList1
--FinishSortSupportFunction
```

#2. get_ordering_op_properties

```
get_ordering_op_properties
--SearchSysCacheList1
----SearchSysCacheList
------SearchCatCacheList
--for (i = 0; i < catlist->n_members; i++)
----HeapTuple	tuple = &catlist->members[i]->tuple;
----Form_pg_amop aform = (Form_pg_amop) GETSTRUCT(tuple);
----if (aform->amopstrategy == BTLessStrategyNumber ||
			aform->amopstrategy == BTGreaterStrategyNumber)
------if (aform->amoplefttype == aform->amoprighttype)
--------*opfamily = aform->amopfamily;
				*opcintype = aform->amoplefttype;
				*strategy = aform->amopstrategy;
				result = true;
--ReleaseSysCacheList(catlist);
----ReleaseCatCacheList
```

#3.FinishSortSupportFunction

```
FinishSortSupportFunction
--sortSupportFunction = get_opfamily_proc(opfamily, opcintype, opcintype,
											BTSORTSUPPORT_PROC);
----SearchSysCache4
------SearchCatCache4
--------SearchCatCacheInternal
----amproc_tup = (Form_pg_amproc) GETSTRUCT(tp);
----result = amproc_tup->amproc;
----ReleaseSysCache(tp);
------ReleaseCatCache
--------CatCacheRemoveCTup
--if (OidIsValid(sortSupportFunction))
----OidFunctionCall1(sortSupportFunction, PointerGetDatum(ssup));
------OidFunctionCall1Coll
--------fmgr_info(functionId, &flinfo);
----------fmgr_info_cxt_security
--------FunctionCall1Coll
----------InitFunctionCallInfoData(*fcinfo, flinfo, 1, collation, NULL, NULL);
----------FunctionCallInvoke(fcinfo);
--if (ssup->comparator == NULL)
----sortFunction = get_opfamily_proc(opfamily, opcintype, opcintype,
										 BTORDER_PROC);
----PrepareSortSupportComparisonShim(sortFunction, ssup);


```