#1._mdfd_openseg

```
_mdfd_openseg
--fullpath = _mdfd_segpath(reln, forknum, segno);
----path = relpath(reln->smgr_rnode, forknum);
----fullpath = psprintf("%s.%u", path, segno);
--fd = PathNameOpenFile
--_fdvec_resize(reln, forknum, segno + 1);
--v = &reln->md_seg_fds[forknum][segno];
--v->mdfd_vfd = fd;
--v->mdfd_segno = segno;

```