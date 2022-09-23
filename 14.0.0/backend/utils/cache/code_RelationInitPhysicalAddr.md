#1.RelationInitPhysicalAddr

```
RelationInitPhysicalAddr
--if (relation->rd_rel->relfilenode)
----if (HistoricSnapshotActive()
			&& RelationIsAccessibleInLogicalDecoding(relation)
			&& IsTransactionState())
------phys_tuple = ScanPgRelation(RelationGetRelid(relation),
					RelationGetRelid(relation) != ClassOidIndexId,true);
------physrel = (Form_pg_class) GETSTRUCT(phys_tuple);
------relation->rd_rel->reltablespace = physrel->reltablespace;
------relation->rd_rel->relfilenode = physrel->relfilenode;
------heap_freetuple(phys_tuple);
----relation->rd_node.relNode = relation->rd_rel->relfilenode;
--else
----relation->rd_node.relNode =
			RelationMapOidToFilenode(relation->rd_id,
									 relation->rd_rel->relisshared);
```