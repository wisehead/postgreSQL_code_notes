#1.mdwrite

```
mdwrite
--_mdfd_getseg
--seekpos = (off_t) BLCKSZ * (blocknum % ((BlockNumber) RELSEG_SIZE));
--nbytes = FileWrite(v->mdfd_vfd, buffer, BLCKSZ, seekpos, WAIT_EVENT_DATA_FILE_WRITE);
--if (!skipFsync && !SmgrIsTemp(reln))
--register_dirty_segment(reln, forknum, v);
```