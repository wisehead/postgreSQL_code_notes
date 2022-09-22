#1.dsm_backend_shutdown

```
dsm_backend_shutdown
--while (!dlist_is_empty(&dsm_segment_list))
----seg = dlist_head_element(dsm_segment, node, &dsm_segment_list);
----dsm_detach(seg);
```