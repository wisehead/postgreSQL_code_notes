#1.smgropen

```
smgropen
--if (SMgrRelationHash == NULL)
----SMgrRelationHash = hash_create
--hash_search
--if (!found)
----smgrsw[reln->smgr_which].smgr_open(reln);
------mdinit
------mdopen
----dlist_push_tail(&unowned_relns, &reln->node)
```