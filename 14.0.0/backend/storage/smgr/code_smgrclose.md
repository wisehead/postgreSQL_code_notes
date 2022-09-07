#1.smgrclose

```cpp
smgrclose
--for (forknum = 0; forknum <= MAX_FORKNUM; forknum++)
----smgrsw[reln->smgr_which].smgr_close(reln, forknum)
------mdclose
--owner = reln->smgr_owner;
--if (!owner)
----dlist_delete(&reln->node);
--hash_search(SMgrRelationHash,
					(void *) &(reln->smgr_rnode),
					HASH_REMOVE, NULL)
```