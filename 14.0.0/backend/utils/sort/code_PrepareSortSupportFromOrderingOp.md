#1.PrepareSortSupportFromOrderingOp

```
PrepareSortSupportFromOrderingOp
--get_ordering_op_properties
----SearchSysCacheList1
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

```