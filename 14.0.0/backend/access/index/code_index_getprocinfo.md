#1.index_getprocinfo

```cpp
index_getprocinfo
--nproc = irel->rd_indam->amsupport;
--optsproc = irel->rd_indam->amoptsprocnum;
--procindex = (nproc * (attnum - 1)) + (procnum - 1);
--locinfo = irel->rd_supportinfo;
--locinfo += procindex;
--if (locinfo->fn_oid == InvalidOid)
----*loc = irel->rd_support;
----procId = loc[procindex];
----fmgr_info_cxt(procId, locinfo, irel->rd_indexcxt);
------fmgr_info_cxt_security(functionId, finfo, mcxt, false);


```