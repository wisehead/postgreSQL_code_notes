#1.dsm_detach

```
dsm_detach
--while (!slist_is_empty(&seg->on_detach))
----node = slist_pop_head_node(&seg->on_detach);
----cb = slist_container(dsm_segment_detach_callback, node, node);
----function = cb->function;
----arg = cb->arg;
----function(seg, arg);
--if (seg->mapped_address != NULL)
----if (!is_main_region_dsm_handle(seg->handle))
------dsm_impl_op(DSM_OP_DETACH, seg->handle, 0, &seg->impl_private,
						&seg->mapped_address, &seg->mapped_size, WARNING);
--if (seg->control_slot != INVALID_CONTROL_SLOT)
----refcnt = --dsm_control->item[control_slot].refcnt;
----seg->control_slot = INVALID_CONTROL_SLOT;
----if (refcnt== 1)
------if (is_main_region_dsm_handle(seg->handle) ||
				dsm_impl_op(DSM_OP_DESTROY, seg->handle, 0, &seg->impl_private,
							&seg->mapped_address, &seg->mapped_size, WARNING))
--------if (is_main_region_dsm_handle(seg->handle))
----------FreePageManagerPut((FreePageManager *) dsm_main_space_begin,
									   dsm_control->item[control_slot].first_page,
									   dsm_control->item[control_slot].npages);
----------dsm_control->item[control_slot].refcnt = 0;
--dlist_delete(&seg->node);
```

