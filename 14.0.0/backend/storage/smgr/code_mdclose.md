#1.mdclose

```cpp
mdclose
--nopensegs = reln->md_num_open_segs[forknum];
--while (nopensegs > 0)
----MdfdVec *v = &reln->md_seg_fds[forknum][nopensegs - 1];
----FileClose(v->mdfd_vfd);
----_fdvec_resize(reln, forknum, nopensegs - 1);
```