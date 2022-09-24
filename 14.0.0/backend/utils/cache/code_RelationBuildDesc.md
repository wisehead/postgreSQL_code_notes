#1.RelationBuildDesc

```
RelationBuildDesc
--pg_class_tuple = ScanPgRelation(targetRelId, true, false);
--relp = (Form_pg_class) GETSTRUCT(pg_class_tuple);
--relid = relp->oid;
--relation = AllocateRelationDesc(relp);
--RelationGetRelid(relation) = relid;
--RelationBuildTupleDesc(relation);
```