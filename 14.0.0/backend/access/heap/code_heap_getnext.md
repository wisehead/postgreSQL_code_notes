#1.heap_getnext

```
heap_getnext
--if (scan->rs_base.rs_flags & SO_ALLOW_PAGEMODE)
----heapgettup_pagemode(scan, direction,
							scan->rs_base.rs_nkeys, scan->rs_base.rs_key);
--else
----heapgettup(scan, direction,
				   scan->rs_base.rs_nkeys, scan->rs_base.rs_key);
```