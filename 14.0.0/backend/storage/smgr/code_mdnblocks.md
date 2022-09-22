#1.mdnblocks

```
mdnblocks
--mdopenfork
--segno = reln->md_num_open_segs[forknum] - 1;
--v = &reln->md_seg_fds[forknum][segno];
--for (;;)
----//Get number of blocks present in a single disk file
----nblocks = _mdnblocks(reln, forknum, v);
----if (nblocks < ((BlockNumber) RELSEG_SIZE))
------return (segno * ((BlockNumber) RELSEG_SIZE)) + nblocks;
----segno++;
----v = _mdfd_openseg(reln, forknum, segno, 0);
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
--mdfd = &reln->md_seg_fds[forknum][0];
--mdfd->mdfd_vfd = fd;
--mdfd->mdfd_segno = 0;
```

#3._fdvec_resize

```
_fdvec_resize
--if (nseg == 0)
----if (reln->md_num_open_segs[forknum] > 0)
------pfree(reln->md_seg_fds[forknum]);
--else if (reln->md_num_open_segs[forknum] == 0)
----reln->md_seg_fds[forknum] =
			MemoryContextAlloc(MdCxt, sizeof(MdfdVec) * nseg);
--else
----reln->md_seg_fds[forknum] =
			repalloc(reln->md_seg_fds[forknum],
					 sizeof(MdfdVec) * nseg);
--reln->md_num_open_segs[forknum] = nseg;
```