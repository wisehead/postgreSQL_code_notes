#1.RelationBuildTupleDesc

```
RelationBuildTupleDesc
--ScanKeyInit
--pg_attribute_desc = table_open(AttributeRelationId, AccessShareLock);
--pg_attribute_scan = systable_beginscan
--need = RelationGetNumberOfAttributes(relation);
----((relation)->rd_rel->relnatts)
--while (systable_getnext(pg_attribute_scan)))
----attp = (Form_pg_attribute) GETSTRUCT(pg_attribute_tuple);
--systable_endscan(pg_attribute_scan);
--table_close(pg_attribute_desc, AccessShareLock);
--relation->rd_att->constr = constr;
--constr->missing = attrmiss;
```