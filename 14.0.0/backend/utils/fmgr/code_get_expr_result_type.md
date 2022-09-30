#1.get_expr_result_type

```
get_expr_result_type
--internal_get_result_type
```

#2.internal_get_result_type

```
internal_get_result_type
--tp = SearchSysCache1(PROCOID, ObjectIdGetDatum(funcid));
--procform = (Form_pg_proc) GETSTRUCT(tp);
--rettype = procform->prorettype;
--tupdesc = build_function_result_tupdesc_t(tp);
--result = get_type_func_class(rettype, &base_rettype);
```

#3.build_function_result_tupdesc_t

```
build_function_result_tupdesc_t
--Form_pg_proc procform = (Form_pg_proc) GETSTRUCT(procTuple);
--SysCacheGetAttr
--build_function_result_tupdesc_d
----CreateTemplateTupleDesc
----for (i = 0; i < numoutargs; i++)
------TupleDescInitEntry
```