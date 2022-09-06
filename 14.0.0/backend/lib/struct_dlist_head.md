#1.struct dlist_head

```cpp
/*
 * Head of a doubly linked list.
 *
 * Non-empty lists are internally circularly linked.  Circular lists have the
 * advantage of not needing any branches in the most common list manipulations.
 * An empty list can also be represented as a pair of NULL pointers, making
 * initialization easier.
 */
typedef struct dlist_head
{
	/*
	 * head.next either points to the first element of the list; to &head if
	 * it's a circular empty list; or to NULL if empty and not circular.
	 *
	 * head.prev either points to the last element of the list; to &head if
	 * it's a circular empty list; or to NULL if empty and not circular.
	 */
	dlist_node	head;
} dlist_head;
```