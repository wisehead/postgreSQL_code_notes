#1.smgrclose

```cpp
smgrclose
--for (forknum = 0; forknum <= MAX_FORKNUM; forknum++)
----smgrsw[reln->smgr_which].smgr_close(reln, forknum)
------mdclose
```