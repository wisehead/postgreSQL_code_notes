#1.mdextend

```
mdextend
--v = _mdfd_getseg
--seekpos = (off_t) BLCKSZ * (blocknum % ((BlockNumber) RELSEG_SIZE));
--FileWrite(v->mdfd_vfd, buffer, BLCKSZ, seekpos, WAIT_EVENT_DATA_FILE_EXTEND))
--if (!skipFsync && !SmgrIsTemp(reln))
----register_dirty_segment(reln, forknum, v);
```