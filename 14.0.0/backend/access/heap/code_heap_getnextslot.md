#1.heap_getnextslot

```
heap_getnextslot
--if (sscan->rs_flags & SO_ALLOW_PAGEMODE)
----heapgettup_pagemode(scan, direction, sscan->rs_nkeys, sscan->rs_key);
--else
----heapgettup(scan, direction, sscan->rs_nkeys, sscan->rs_key);
```