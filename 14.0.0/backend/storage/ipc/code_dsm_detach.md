#1.dsm_detach

```
dsm_detach
--while (!slist_is_empty(&seg->on_detach))
----node = slist_pop_head_node(&seg->on_detach);
```