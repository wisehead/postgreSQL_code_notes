#1.RelationBuildDesc

```
RelationBuildDesc
--pg_class_tuple = ScanPgRelation(targetRelId, true, false);
--relp = (Form_pg_class) GETSTRUCT(pg_class_tuple);
--relid = relp->oid;
--relation = AllocateRelationDesc(relp);
--RelationGetRelid(relation) = relid;
--RelationBuildTupleDesc(relation);
--if (relation->rd_rel->relhasrules)
----RelationBuildRuleLock(relation);
--if (relation->rd_rel->relhastriggers)
----RelationBuildTriggers(relation);
--if (relation->rd_rel->relrowsecurity)
----RelationBuildRowSecurity(relation);
--switch (relation->rd_rel->relkind)
----case RELKIND_INDEX:
------RelationInitIndexAccessInfo
----case RELKIND_RELATION:
------RelationInitTableAccessMethod
--RelationParseRelOptions
```