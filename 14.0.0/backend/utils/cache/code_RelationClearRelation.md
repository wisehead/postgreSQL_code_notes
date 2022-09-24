#1.RelationClearRelation

```
RelationClearRelation
--RelationCloseSmgr(relation);
--if (relation->rd_isnailed)
----RelationReloadNailed
--if ((relation->rd_rel->relkind == RELKIND_INDEX ||
		 relation->rd_rel->relkind == RELKIND_PARTITIONED_INDEX) &&
		relation->rd_refcnt > 0 &&
		relation->rd_indexcxt != NULL)
----RelationReloadIndexInfo(relation);
--if (!rebuild)
----RelationCacheDelete(relation);
----RelationDestroyRelation(relation, false);
------RelationCloseSmgr(relation);
--else if (!IsTransactionState())
----return;
--else
----newrel = RelationBuildDesc(save_relid, false);
```