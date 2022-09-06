#1. index_getprocinfo

```cpp

/* ----------------
 *      index_getprocinfo
 *
 *      This routine allows index AMs to keep fmgr lookup info for
 *      support procs in the relcache.  As above, only the "default"
 *      functions for any particular indexed attribute are cached.
 *
 * Note: the return value points into cached data that will be lost during
 * any relcache rebuild!  Therefore, either use the callinfo right away,
 * or save it only after having acquired some type of lock on the index rel.
 * ----------------
 */
FmgrInfo *
index_getprocinfo(Relation irel,
                  AttrNumber attnum,
                  uint16 procnum)
```