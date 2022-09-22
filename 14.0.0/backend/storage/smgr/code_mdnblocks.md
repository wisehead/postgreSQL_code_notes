#1.mdnblocks

```
mdnblocks
--mdopenfork
--
```

#2. mdopenfork

```
mdopenfork
--if (reln->md_num_open_segs[forknum] > 0)
----return &reln->md_seg_fds[forknum][0];
--path = relpath(reln->smgr_rnode, forknum);
----relpathbackend((rnode).node, (rnode).backend, forknum)
------GetRelationPath
--PathNameOpenFile(path, O_RDWR | PG_BINARY);
----PathNameOpenFilePerm(fileName, fileFlags, pg_file_create_mode);
--pfree(path);
--_fdvec_resize(reln, forknum, 1);
```

#3._fdvec_resize

```
_fdvec_resize
--
```