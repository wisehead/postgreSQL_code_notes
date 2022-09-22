#1._mdfd_getseg

```
_mdfd_getseg
--targetseg = blkno / ((BlockNumber) RELSEG_SIZE);
--if (targetseg < reln->md_num_open_segs[forknum])
----v = &reln->md_seg_fds[forknum][targetseg];
----return v;
--if (reln->md_num_open_segs[forknum] > 0)
----v = &reln->md_seg_fds[forknum][reln->md_num_open_segs[forknum] - 1];
--else
----v = mdopenfork(reln, forknum, behavior);
--for (nextsegno = reln->md_num_open_segs[forknum];nextsegno <= targetseg; nextsegno++)
----nblocks = _mdnblocks(reln, forknum, v);
----if ((behavior & EXTENSION_CREATE) ||
			(InRecovery && (behavior & EXTENSION_CREATE_RECOVERY)))
------if (nblocks < ((BlockNumber) RELSEG_SIZE))
--------zerobuf = palloc0(BLCKSZ);
--------mdextend(reln, forknum,
						 nextsegno * ((BlockNumber) RELSEG_SIZE) - 1,
						 zerobuf, skipFsync);
----else if (!(behavior & EXTENSION_DONT_CHECK_SIZE) &&
				 nblocks < ((BlockNumber) RELSEG_SIZE))
------ereport
----v = _mdfd_openseg(reln, forknum, nextsegno, flags);//open new segment
```